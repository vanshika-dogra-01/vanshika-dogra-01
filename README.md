{{ 'component-card.css' | asset_url | stylesheet_tag }}
{{ 'component-price.css' | asset_url | stylesheet_tag }}
{{ 'component-rte.css' | asset_url | stylesheet_tag }}
<link rel="stylesheet" href="{{ 'component-slider.css' | asset_url }}" media="print" onload="this.media='all'">
<link rel="stylesheet" href="{{ 'template-collection.css' | asset_url }}" media="print" onload="this.media='all'">
{%- if section.settings.enable_quick_add -%}
  <link rel="stylesheet" href="{{ 'quick-add.css' | asset_url }}" media="print" onload="this.media='all'">
  <script src="{{ 'quick-add.js' | asset_url }}" defer="defer"></script>
  <script src="{{ 'product-form.js' | asset_url }}" defer="defer"></script>
{%- endif -%}
<noscript>{{ 'component-slider.css' | asset_url | stylesheet_tag }}</noscript>
<noscript>{{ 'template-collection.css' | asset_url | stylesheet_tag }}</noscript>
{%- style -%}
  .section-{{ section.id }}-padding {
    padding-top: {{ section.settings.padding_top | times: 0.75 | round: 0 }}px;
    padding-bottom: {{ section.settings.padding_bottom | times: 0.75 | round: 0 }}px;
  }
  @media screen and (min-width: 750px) {
    .section-{{ section.id }}-padding {
      padding-top: {{ section.settings.padding_top }}px;
      padding-bottom: {{ section.settings.padding_bottom }}px;
    }
  }
{%- endstyle -%}
<div class="feature-product-tab collection feature-collection color-{{ section.settings.color_scheme }} isolate gradient ">
  <div class="collection tab-sections page-width section-{{ section.id }}-padding{% if section.settings.full_width %} collection--full-width{% endif %}">
    <!-- <div class="collection__title center title-wrapper title-wrapper--no-top-margin page-width">
      {%- if section.settings.title != blank -%}
        <h2 class="title inline-richtext {{ section.settings.heading_size }}{% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}">{{ section.settings.title | escape }}</h2>
      {%- endif -%}
      {%- if section.settings.description != blank
        or section.settings.show_description
        and section.settings.collection.description != empty
      -%}
        <div class="collection__description {{ section.settings.description_style }} rte">
          {%- if section.settings.show_description -%}
            {{ section.settings.collection.description }}
          {%- else -%}
            {{ section.settings.description -}}
          {%- endif %}
        </div>
      {%- endif -%}
    </div> -->
    <div class="feattab tab">
  <div class="tabs-nav">
        <div class="collection__title center title-wrapper title-wrapper--no-top-margin ">
      {%- if section.settings.title != blank -%}
        <h2 class="title inline-richtext {{ section.settings.heading_size }}{% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}">{{ section.settings.title | escape }}</h2>
      {%- endif -%}
      {%- if section.settings.description != blank
        or section.settings.show_description
        and section.settings.collection.description != empty
      -%}
        <div class="collection__description {{ section.settings.description_style }} rte">
          {%- if section.settings.show_description -%}
            {{ section.settings.collection.description }}
          {%- else -%}
            {{ section.settings.description -}}
          {%- endif %}
        </div>
      {%- endif -%}
    </div>
    <div class="tab-block">
    {%- for block in section.blocks -%}
    <a href="#tab-{{ forloop.index }}">
      {{ block.settings.coll_name }}</a>
      {%- endfor -%}
    </div>
  </div>
  <div class="tabs-stage">
    {%- for block in section.blocks -%}
{%- liquid
  assign products_to_display = block.settings.collection.all_products_count
  if block.settings.collection.all_products_count > block.settings.products_to_show
    assign products_to_display = block.settings.products_to_show
    assign more_in_collection = true
  endif
  assign columns_mobile_int = block.settings.columns_mobile | plus: 0
  assign show_mobile_slider = false
  if block.settings.swipe_on_mobile and products_to_display > columns_mobile_int
    assign show_mobile_slider = true
  endif
  assign show_desktop_slider = false
  if block.settings.enable_desktop_slider and products_to_display > block.settings.columns_desktop
    assign show_desktop_slider = true
  endif
-%}
    <div id="tab-{{ forloop.index }}" class="tabsbox">
     <slider-component class="slider-mobile-gutter{% if block.settings.full_width %} slider-component-full-width{% endif %}{% if show_mobile_slider == false %} page-width{% endif %}{% if show_desktop_slider == false and block.settings.full_width == false %} page-width-desktop{% endif %}{% if show_desktop_slider %} slider-component-desktop{% endif %}{% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}">
       <ul
        id="Slider-{{ section.id }}"
        class="custom-grid grid product-grid contains-card contains-card--product{% if settings.card_style == 'standard' %} contains-card--standard{% endif %} grid--{{ block.settings.columns_desktop }}-col-desktop{% if block.settings.collection == blank %} grid--2-col-tablet-down{% else %} grid--{{ block.settings.columns_mobile }}-col-tablet-down{% endif %}{% if show_mobile_slider or show_desktop_slider %} slider{% if show_desktop_slider %} slider--desktop{% endif %}{% if show_mobile_slider %} slider--tablet grid--peek{% endif %}{% endif %}"
        role="list"
        aria-label="{{ 'general.slider.name' | t }}"
      >
          {%- for product in block.settings.collection.products limit: block.settings.products_to_show -%}
            <li
            id="Slide-{{ section.id }}-{{ forloop.index }}"
            class="grid__item{% if show_mobile_slider or show_desktop_slider %} slider__slide{% endif %}{% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}"
            {% if settings.animations_reveal_on_scroll %}
              data-cascade
              style="--animation-order: {{ forloop.index }};"
            {% endif %}
          >
              {% render 'card-product',
                card_product: product,
                media_aspect_ratio: section.settings.image_ratio,
                show_secondary_image: section.settings.show_secondary_image,
                show_vendor: section.settings.show_vendor,
                show_rating: section.settings.show_rating,
                show_quick_add: section.settings.enable_quick_add,
                section_id: section.id
              %}
            </li>
          {%- else -%}
            {%- for i in (1..4) -%}
              <div class="grid__item">
                {% render 'card-product', show_vendor: section.settings.show_vendor %}
              </div>
            {%- endfor -%}
          {%- endfor -%}
        </ul>
        {%- if show_mobile_slider or show_desktop_slider -%}
          <div class="slider-buttons no-js-hidden">
            <button
              type="button"
              class="slider-button slider-button--prev"
              name="previous"
              aria-label="{{ 'general.slider.previous_slide' | t }}"
              aria-controls="Slider-{{ section.id }}"
            >
               <span class="svg-wrapper">
              {{- 'icon-caret.svg' | inline_asset_content -}}
            </span>
            </button>
            <div class="slider-counter caption">
              <span class="slider-counter--current">1</span>
              <span aria-hidden="true"> / </span>
              <span class="visually-hidden">{{ 'general.slider.of' | t }}</span>
              <span class="slider-counter--total">{{ products_to_display }}</span>
            </div>
            <button
              type="button"
              class="slider-button slider-button--next"
              name="next"
              aria-label="{{ 'general.slider.next_slide' | t }}"
              aria-controls="Slider-{{ section.id }}"
            >
              <span class="svg-wrapper">
              {{- 'icon-caret.svg' | inline_asset_content -}}
            </span>
            </button>
          </div>
        {%- endif -%}
      </slider-component>
      {%- if block.settings.show_view_all -%}
      <div class="center collection__view-all">
        <a
          href="{{ block.settings.collection.url }}"
          class="{% if section.settings.view_all_style == 'link' %}link underlined-link{% elsif section.settings.view_all_style == 'solid' %}button{% else %}button button--secondary{% endif %}"
          aria-label="{{ 'sections.featured_collection.view_all_label' | t: collection_name: block.settings.collection.title }}"
        >
          {{ 'sections.featured_collection.view_all' | t }}
        </a>
      </div>
    {%- endif -%}
    </div>
    {%- endfor -%}
  </div>
</div>

    <div class="discover-store-btn">
<a href="{{ section.settings.discover-btn-url }}">
{{ section.settings.discover-btn-text }}
</a>
    </div>
  </div>


</div>
<script>
// Tab collection section on homepage js
$('.tabs-stage .tabsbox').hide();
$('.tabs-stage .tabsbox:first').show();
$('.tabs-nav a:first').addClass('tab-active');
// Change tab class and display content
$('.tabs-nav a').on('click', function(event){
  event.preventDefault();
  $('.tabs-nav a').removeClass('tab-active'); // Remove tab-active class from all tabs
  $(this).addClass('tab-active'); // Add tab-active class to the clicked tab
  $('.tabs-stage .tabsbox').hide();
  $($(this).attr('href')).show();
});
// End Tab collection section on homepage js
</script>
{% schema %}
{
  "name": "Featured Tab Collections",
  "tag": "section",
  "class": "section",
  "disabled_on": {
    "groups": ["header", "footer"]
  },
  "settings": [
    {
      "type": "text",
      "id": "title",
      "default": "Featured collection",
      "label": "t:sections.featured-collection.settings.title.label"
    },
    
    {
      "type": "select",
      "id": "heading_size",
      "options": [
        {
          "value": "h2",
          "label": "t:sections.all.heading_size.options__1.label"
        },
        {
          "value": "h1",
          "label": "t:sections.all.heading_size.options__2.label"
        },
        {
          "value": "h0",
          "label": "t:sections.all.heading_size.options__3.label"
        }
      ],
      "default": "h1",
      "label": "t:sections.all.heading_size.label"
    },
    {
      "type": "richtext",
      "id": "description",
      "label": "t:sections.featured-collection.settings.description.label"
    },
    {
      "type": "checkbox",
      "id": "show_description",
      "label": "t:sections.featured-collection.settings.show_description.label",
      "default": false
    },
    {
      "type": "select",
      "id": "description_style",
      "label": "t:sections.featured-collection.settings.description_style.label",
      "options": [
        {
          "value": "body",
          "label": "t:sections.featured-collection.settings.description_style.options__1.label"
        },
        {
          "value": "subtitle",
          "label": "t:sections.featured-collection.settings.description_style.options__2.label"
        },
        {
          "value": "uppercase",
          "label": "t:sections.featured-collection.settings.description_style.options__3.label"
        }
      ],
      "default": "body"
    },
    {
      "type": "select",
      "id": "view_all_style",
      "label": "t:sections.featured-collection.settings.view_all_style.label",
      "options": [
        {
          "value": "link",
          "label": "t:sections.featured-collection.settings.view_all_style.options__1.label"
        },
        {
          "value": "outline",
          "label": "t:sections.featured-collection.settings.view_all_style.options__2.label"
        },
        {
          "value": "solid",
          "label": "t:sections.featured-collection.settings.view_all_style.options__3.label"
        }
      ],
      "default": "solid"
    },
    {
      "type": "url",
      "id": "discover-btn-url",
      "label": "Button Link"
    },
    {
      "type": "text",
      "id": "discover-btn-text",
      "label": "Button Text"
    },
    {
      "type": "color_scheme",
      "id": "color_scheme",
      "label": "t:sections.all.colors.label",
      "default": "scheme-1"
    },
    {
      "type": "header",
      "content": "t:sections.featured-collection.settings.header.content"
    },
    {
      "type": "select",
      "id": "image_ratio",
      "options": [
        {
          "value": "adapt",
          "label": "t:sections.featured-collection.settings.image_ratio.options__1.label"
        },
        {
          "value": "portrait",
          "label": "t:sections.featured-collection.settings.image_ratio.options__2.label"
        },
        {
          "value": "square",
          "label": "t:sections.featured-collection.settings.image_ratio.options__3.label"
        }
      ],
      "default": "adapt",
      "label": "t:sections.featured-collection.settings.image_ratio.label"
    },
    {
      "type": "checkbox",
      "id": "show_secondary_image",
      "default": false,
      "label": "t:sections.featured-collection.settings.show_secondary_image.label"
    },
    {
      "type": "checkbox",
      "id": "show_vendor",
      "default": false,
      "label": "t:sections.featured-collection.settings.show_vendor.label"
    },
    {
      "type": "checkbox",
      "id": "show_rating",
      "default": false,
      "label": "t:sections.featured-collection.settings.show_rating.label",
      "info": "t:sections.featured-collection.settings.show_rating.info"
    },
    {
      "type": "checkbox",
      "id": "enable_quick_add",
      "default": false,
      "label": "t:sections.featured-collection.settings.enable_quick_buy.label"
    },
      {
      "type": "checkbox",
      "id": "enable_add_to_cart",
      "default": false,
      "label": "Add to cart"
    },
    {
      "type": "select",
      "id": "quick_add",
      "default": "none",
      "label": "t:sections.main-collection-product-grid.settings.quick_add.label",
      "info": "t:sections.main-collection-product-grid.settings.quick_add.info",
      "options": [
        {
          "value": "none",
          "label": "t:sections.main-collection-product-grid.settings.quick_add.options.option_1"
        },
        {
          "value": "standard",
          "label": "t:sections.main-collection-product-grid.settings.quick_add.options.option_2"
        },
        {
          "value": "bulk",
          "label": "t:sections.main-collection-product-grid.settings.quick_add.options.option_3"
        }
      ]
    },
    {
      "type": "header",
      "content": "t:sections.featured-collection.settings.header_mobile.content"
    },
    {
      "type": "header",
      "content": "t:sections.all.padding.section_padding_heading"
    },
    {
      "type": "range",
      "id": "padding_top",
      "min": 0,
      "max": 100,
      "step": 4,
      "unit": "px",
      "label": "t:sections.all.padding.padding_top",
      "default": 36
    },
    {
      "type": "range",
      "id": "padding_bottom",
      "min": 0,
      "max": 100,
      "step": 4,
      "unit": "px",
      "label": "t:sections.all.padding.padding_bottom",
      "default": 36
    },
    {
      "type": "color",
      "id": "section_bg_color_2",
      "label": "Title Color",
      "default": "#000"
    },
       {
      "type": "color",
      "id": "slide_text_colors",
      "label": "Tab Background",
      "default": "#000"
    },
    {
      "type": "color",
      "id": "slide_text_color",
      "label":  "Tabs Active Background",
      "default": "#000"
    },
    {
      "type": "color",
      "id": "txts_colors",
      "label":"Tabs Active Color",
      "default": "#000"
    }
  ],
  "blocks": [
    {
      "type": "collection",
      "name": "Collection",
      "settings": [
      {
        "type": "collection",
        "id": "collection",
        "label": "t:sections.featured-collection.settings.collection.label"
      },
      {
        "type": "text",
        "id": "coll_name",
        "label": "Collection Name"
      },
      {
        "type": "range",
        "id": "products_to_show",
        "min": 2,
        "max": 25,
        "step": 1,
        "default": 4,
        "label": "t:sections.featured-collection.settings.products_to_show.label"
      },
 {
        "type": "checkbox",
        "id": "full_width",
        "label": "t:sections.featured-collection.settings.full_width.label",
        "default": false
      },
      {
        "type": "range",
        "id": "columns_desktop",
        "min": 1,
        "max": 5,
        "step": 1,
        "default": 4,
        "label": "t:sections.featured-collection.settings.columns_desktop.label"
      },
    {
      "type": "checkbox",
      "id": "enable_desktop_slider",
      "label": "t:sections.featured-collection.settings.enable_desktop_slider.label",
      "default": false
    },
{
      "type": "select",
      "id": "columns_mobile",
      "default": "2",
      "label": "t:sections.featured-collection.settings.columns_mobile.label",
      "options": [
        {
          "value": "1",
          "label": "t:sections.featured-collection.settings.columns_mobile.options__1.label"
        },
        {
          "value": "2",
          "label": "t:sections.featured-collection.settings.columns_mobile.options__2.label"
        }
      ]
    },
    {
      "type": "checkbox",
      "id": "swipe_on_mobile",
      "default": false,
      "label": "t:sections.featured-collection.settings.swipe_on_mobile.label"
    },
      {
        "type": "checkbox",
        "id": "show_view_all",
        "default": true,
        "label": "t:sections.featured-collection.settings.show_view_all.label"
      }
      ]
    }
  ],
  "presets": [
    {
      "name": "Featured Tab Collections"
    }
  ]
}
{% endschema %}
