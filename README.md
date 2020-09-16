<img src="https://img.imageboss.me/boss-images/width/180/dpr:2/emails/logo-2@2x.png" width="180"/>

# ImageBoss for Shopify

This is a step-by-step guide on how to integrate ImageBoss with Shopify.

## Installation

### Add Theme Settings

1. Login into your Shopify account. Go to Online Store > Themes > Current Theme > Actions > Edit Code.
2. Go to the `Config` section > Edit the `settings_schema.json`.

As you can see, this is a array of configs.
```json
[
  { ... },
  { ... },
]
```

So at the end of this file, right after the last `}` character, add a comma, like this `},` and add:

```json
{
  "name": "ImageBoss",
  "settings": [
    {
      "type": "paragraph",
      "content": "For more information see the [docs](https:\/\/imageboss.me/docs\/integrations\/shopify)"
    },
    {
      "type": "checkbox",
      "id": "imageboss_enable",
      "label": "Enable ImageBoss",
      "info": "Toggle the ImageBoss feature"
    },
    {
      "type": "text",
      "id": "imageboss_source",
      "label": "ImageBoss Source",
      "default": "my-website-source",
      "info": "The ImageBoss source you get from your Dashboard"
    }
  ]
}

```

3. Now upload the ImageBoss helper into the `Snippets` section called `imageboss.liquid`. The content of the file is this:

```liquid
{% capture image_url %}
    {% if settings.imageboss_enable %}
      {% assign imageboss_api_url = "https://img.imageboss.me" %}
      {% assign src_without_domain = src | split: '//cdn.shopify.com' %}
      {% assign base_url = imageboss_api_url | append: '/' | append: settings.imageboss_source %}
      {% assign params = operation %}

      {% if size %}
        {% assign params = params | append: '/' | append: size %}
      {% endif %}
      {% if options %}
        {% assign params = params | append: '/' | append: options %}
      {% endif %}
      {{ base_url }}/{{ params }}{{ src_without_domain[1] }}
    {% else %}
      {{ src }}
    {% endif %}
{% endcapture %}{{ image_url | strip }}
```

4. Now go to your Theme Settings and activate the ImageBoss helper.

Basic Usage
===========

As each layout/theme has its own complexities, you need to edit your themes and modifiy the original URLs of the images as follows:
```liquid
{% assign  product_url = image | img_url: 'master' %}
{% capture imageboss_url %}{% include 'imageboss' src:product_url, operation: 'cover', size: '300x300', options: 'dpr:2' %}{% endcapture %}
<img src="{{ imageboss_url }}" />
```

See more examples opening the `examples/` folder.

Read the full [ImageBoss API Reference](https://imageboss.me/docs)

## License

[The MIT License](LICENSE).
