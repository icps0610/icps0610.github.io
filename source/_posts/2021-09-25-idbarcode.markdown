---
layout: post
title: "IDBarcode"
date: 2021-09-25 17:07:49 +0800
comments: true
categories: golang
---

``` go
package main

import (
    "github.com/harry1453/go-common-file-dialog/cfd"
    "github.com/harry1453/go-common-file-dialog/cfdutil"
    "github.com/makiuchi-d/gozxing"
    "github.com/makiuchi-d/gozxing/oned"
    "github.com/xuri/excelize/v2"

    "fmt"
    "image/png"
    "os"
    "os/exec"
    "path/filepath"
    "runtime"
    "strconv"
    "strings"
    "syscall"
    "unsafe"
)

var (
    temp = "temp"
)

func main() {
    targetPath := openFile()
    targetDir := filepath.Dir(targetPath) + "\\"

    targetFileName := filepath.Base(targetPath)

    outputPath := targetDir + "barcode_" + targetFileName

    cmd := fmt.Sprintf(`mkdir %s`, temp)
    runCmd(cmd)

    f, err := excelize.OpenFile(targetPath)
    printError(err)

    sheet := f.GetSheetMap()[1]
    rows, _ := f.GetRows(sheet)
    for i, row := range rows {
        if len(row) > 1 {
            id := row[1]
            if checkIDCode(id) != "" {
                f.SetRowHeight(sheet, i+1, 30)
                genCode128(id)
            }
        }
    }

    for i, row := range rows {
        for j, cell := range row {
            l := rune(j + 65 + 2)
            location := fmt.Sprintf("%c%v", l, i+1)
            if j == 1 {
                f.AddPicture(sheet, location, genPath(cell), `{"y_offset": 6, "x_scale": 0.5, "y_scale": 0.5, "positioning": "oneCell"}`)
            }
        }
    }
    cmd = fmt.Sprintf(`del /q %s`, temp)
    runCmd(cmd)

    cmd = fmt.Sprintf(`rmdir %s`, temp)
    runCmd(cmd)

    err = f.SaveAs(outputPath)
    if err == nil {
        //MessageBox("產生", "檔案已產生  " + outputPath)
        cmd = fmt.Sprintf(`explorer /select, %s`, outputPath)
        runCmd(cmd)
    } else {
        MessageBox("錯誤 !!!", err.Error())
    }

}

func checkIDCode(num string) string {
    var sum int
    idTableCh := strings.Split("FBEAHDNTPJKQMGOCUIVWXZ", "")
    idTableValue := []int{15, 11, 14, 10, 17, 13, 22, 27, 23, 18, 19, 24, 21, 16, 35, 12, 28, 34, 29, 32, 30, 33}

    for idx, c := range idTableCh {
        if c == string(num[:1]) {
            v := idTableValue[idx]
            s := (v%10)*9 + (v / 10)
            sum += s
            break
        }
    }

    for idx, n := range num[1:9] {
        i, _ := strconv.Atoi(string(n))
        sum += i * (8 - idx)
    }
    i, _ := strconv.Atoi(string(num[9:]))
    sum += i
    if sum%10 == 0 {
        return num
    }
    return ""
}

func openFile() string {
    result, _ := cfdutil.ShowOpenFileDialog(cfd.DialogConfig{
        Title: "Open A xlsx File",
        Role:  "Open A xlsx File",
        FileFilters: []cfd.FileFilter{
            {
                DisplayName: "*.xlsx",
                Pattern:     "*.xlsx",
            },
            {
                DisplayName: "All Files (*.*)",
                Pattern:     "*.*",
            },
        },
        SelectedFileFilterIndex: 2,
        FileName:                "*.xlsx",
        DefaultExtension:        "xlsx",
    })

    return result
}

func genCode128(id string) {
    enc := oned.NewCode128Writer()
    img, _ := enc.Encode(id, gozxing.BarcodeFormat_CODE_128, 250, 50, nil)
    path := genPath(id)
    file, _ := os.Create(path)
    defer file.Close()
    png.Encode(file, img)
}

func genPath(id string) string {
    return fmt.Sprintf(`%s\%s.png`, temp, id)
}

func MessageBox(title, caption string) {
    syscall.NewLazyDLL("user32.dll").NewProc("MessageBoxW").Call(
        uintptr(0),
        uintptr(unsafe.Pointer(syscall.StringToUTF16Ptr(caption))),
        uintptr(unsafe.Pointer(syscall.StringToUTF16Ptr(title))),
        uintptr(0))
}

func runCmd(cmd string) {
    exec.Command("cmd.exe", "/C", cmd).CombinedOutput()
}

func printError(err error) {
    if err != nil {
        pc, _, _, _ := runtime.Caller(1)
        fmt.Println(runtime.FuncForPC(pc).Name())
        fmt.Println(err)
    }
}

```