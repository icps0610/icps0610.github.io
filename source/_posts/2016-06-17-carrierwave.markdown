---
layout: post
title: "Carrierwave"
date: 2016-06-17 00:31:49 +0800
comments: true
categories: ror
---
<pre><code>#Gemfile
gem 'carrierwave'
gem 'rmagick'
</code></pre>
 
`apt-get imagemagick`  
`rails g uploader image`  
`rails g migration add_image_to_stores`  
 
``` ruby
#app/uploaders/image_uploader.rb
include CarrierWave::RMagick
version :thumb do
   process :convert => 'png'
   process :resize_to_limit => [50, 50]
end

def extension_white_list
   %w(jpg jpeg gif png)
end

def filename
  "#{model.id}.png" if original_filename
end
```
  
``` ruby
#public\uploads\store\image.rb
def store_dir
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
end
 
def default_url
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/" + [version_name, "default.png"].compact.join('_')
end
```
 
 
``` ruby
#XXXXXXXXX_add_image_to_stores.rb
class AddImageToStores < ActiveRecord::Migration
  def change
  	add_column :stores, :image, :string
  end
end
```
 
``` ruby
#model
class Store < ActiveRecord::Base
	mount_uploader :image, ImageUploader
end
```
 
``` ruby
#cotroller
private
 
def store_params
  params.require(:store).permit(:name, :description,:image)
end
```
 
``` ruby
#view
<%= image_tag @store.image_url.to_s %>
<%= image_tag @store.image_url(:thumb).to_s %>
```

### content_type

``` ruby
#app/models/share.rb
class Share < ApplicationRecord
  mount_uploader :image, ImageUploader

  before_save :update_image_attributes

  private

  def update_image_attributes
    if image.present? && image_changed?
      self.content_type = image.file.content_type
    end
  end
end

#db  =>>> t.string :content_type
```