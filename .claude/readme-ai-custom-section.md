# Custom Section Format Rules

When I ask you to create any custom Shopify section, always follow
my custom section format below.

You must create the section using Shopify Liquid, schema, CSS, and JS if required.

Main rules:

1. File naming (kebab-case):
- Section file: banner-with-text.liquid
- CSS asset file: banner-with-text.css

2. Always include CSS asset at the top of the liquid file:
{{ 'section-name.css' | asset_url | stylesheet_tag }}

3. Always include this dynamic spacing structure:
- Desktop margin top / bottom
- Desktop padding top / bottom
- Mobile padding top / bottom

4. Always use this scoped style block:
{%- style -%}
  @media screen and (min-width: 750px) {
    #shopify-section-{{ section.id }} {
      margin-top: {{ section.settings.margin_top }}px;
      margin-bottom: {{ section.settings.margin_bottom }}px;
    }
    .section-{{ section.id }}-padding {
      padding-top: {{ section.settings.padding_top }}px;
      padding-bottom: {{ section.settings.padding_bottom }}px;
    }
  }
  @media screen and (max-width: 749px) {
    .section-{{ section.id }}-padding {
      padding-top: {{ section.settings.padding_top_mobi }}px;
      padding-bottom: {{ section.settings.padding_bottom_mobi }}px;
    }
  }
{%- endstyle -%}

5. Main wrapper format (always use this):
<div class="section-{{ section.id }} section-{{ section.id }}-padding section-{{ section.id }}-margin color-{{ section.settings.color_scheme }} gradient section-main-box CUSTOM-SECTION-CLASS">
  <!-- section content here -->
</div>

Replace CUSTOM-SECTION-CLASS with the section-specific wrapper class.
Example: banner-with-text-main

6. Schema must always include:
- name, class, tag: section, settings, presets
- Schema class must match the section wrapper name

Example: "class": "banner-with-text-wrapper"

7. Always include color_scheme setting first:
{
  "type": "color_scheme",
  "id": "color_scheme",
  "label": "Color scheme",
  "default": "background-1"
}

8. Always include this desktop spacing group:
{
  "type": "header",
  "content": "Section Settings"
},
{
  "type": "range",
  "id": "padding_top",
  "min": 0,
  "max": 200,
  "step": 2,
  "unit": "px",
  "label": "Padding Top",
  "default": 0
},
{
  "type": "range",
  "id": "padding_bottom",
  "min": 0,
  "max": 200,
  "step": 2,
  "unit": "px",
  "label": "Padding Bottom",
  "default": 0
},
{
  "type": "range",
  "id": "margin_top",
  "min": 0,
  "max": 200,
  "step": 2,
  "unit": "px",
  "label": "Margin Top",
  "default": 0
},
{
  "type": "range",
  "id": "margin_bottom",
  "min": 0,
  "max": 200,
  "step": 2,
  "unit": "px",
  "label": "Margin Bottom",
  "default": 0
}

9. Always include this mobile spacing group:
{
  "type": "header",
  "content": "Section Settings For Mobile"
},
{
  "type": "range",
  "id": "padding_top_mobi",
  "min": 0,
  "max": 100,
  "step": 1,
  "unit": "px",
  "label": "Padding top",
  "default": 25
},
{
  "type": "range",
  "id": "padding_bottom_mobi",
  "min": 0,
  "max": 100,
  "step": 1,
  "unit": "px",
  "label": "Padding bottom",
  "default": 25
}

10. CSS format rules:
- One-line per selector (no multi-line property blocks)
- Each new CSS class starts on a new line
- Group same-breakpoint responsive rules in one shared @media block
- Use wrapper-first scoped architecture

CSS example:
.banner-with-text-main { width: 100%; position: relative; }
.banner-with-text-main .banner-inner { display: flex; align-items: center; gap: 40px; }
.banner-with-text-main .banner-content { width: 50%; }
@media screen and (max-width: 749px) {
.banner-with-text-main .banner-inner { flex-direction: column; gap: 20px; }
.banner-with-text-main .banner-content { width: 100%; }
}

11. JavaScript rules:
- Add JS only if required
- Scope inside section using section id or wrapper class
- Do not affect other sections
- Support multiple section instances (no conflicts)

12. Section heading structure (always use this):
{% if section.settings.heading != blank or section.settings.sub_title != blank %}
  <div class="sec-head text-align-{{ section.settings.text_align }} {% if settings.animations_reveal_on_scroll %} scroll-trigger animate--slide-in{% endif %}">

    {% if section.settings.heading != blank %}
      <h2 class="sec-title">{{ section.settings.heading }}</h2>
    {% endif %}

    {% if section.settings.sec_text != blank %}
      <div class="sec-text">{{ section.settings.sec_text }}</div>
    {% endif %}

  </div>
{% endif %}

13. Heading schema settings (always include these):
{
  "type": "inline_richtext",
  "id": "heading",
  "label": "Heading",
  "default": "Section heading"
},
{
  "type": "richtext",
  "id": "sec_text",
  "label": "Content",
  "default": "<p>Add section content here</p>"
},
{
  "type": "select",
  "id": "text_align",
  "label": "Text alignment",
  "default": "center",
  "options": [
    {
      "value": "left",
      "label": "Left"
    },
    {
      "value": "center",
      "label": "Center"
    },
    {
      "value": "right",
      "label": "Right"
    }
  ]
}

14. Extra settings — add based on section needs:
- image_picker for images
- text for simple single-line values
- inline_richtext for headings / labels
- richtext for descriptions / long content
- url for button links
- collection / product if needed
- checkbox / select / range / color where useful

Important: Schema JSON must always be formatted as expanded multi-line objects.
Do not write schema settings, blocks, presets, or options as one-line objects.

When I say "create custom section", use this full format automatically.