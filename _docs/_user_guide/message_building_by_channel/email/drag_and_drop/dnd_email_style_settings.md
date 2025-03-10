---
nav_title: Email Global Style Settings
article_title: Email Global Style Settings
alias: "/dnd/global_style_settings/"
channel: email
page_order: 3
description: "This reference article covers how to set global email style settings for your campaigns and Canvases."
tool: 
  - Campaigns
  - Canvas
---

# Drag & Drop Editor settings

With global style settings, you can personalize the look of your email campaigns and Canvases. You can add and customize a default theme for your Drag & Drop Editor. This includes editing your styles for email titles, text, buttons, and more.

To edit your global style settings, go to the **Manage Settings** page and select the **Email Settings** tab. Select **Drag & Drop Email Editor Settings**.

![][1]

{% alert note %}
Updates made to the global style settings will apply to all future email campaigns and Canvases. 
{% endalert %} 

## Basic styling 

In the **Basic Styling** dropdown, you can set your default email and content background colors for your email campaigns and Canvases. You can also select a default font, add a custom font, and edit link colors.

![Basic styling options that include options to edit the email and content background colors, default font name, and default link color.][2]

### Custom font

With custom fonts, you can manually add a web font for branding consistency across various email platforms. Currently, you can only add one custom font per section (Basic, Title, and Text).

To add a custom font:

1. Click **Add a custom font**.
2. Enter the font's name and source file URL. This source file URL must point to a style sheet like a CSS file. For the **Font Name** field, enter the same font name as your custom font source file. Ensure that the name is capitalized and spaced correctly. Enter the corresponding **Font URL**. 
3. Check that the preview shows your custom font before saving. 
4. Click **Save** to use the custom font as your default email font. 

{% alert important %}
Gmail does not support custom fonts, so your custom font may display as a default system font. For other email platforms, check that your custom font displays correctly prior to sending your email messaging.
{% endalert %}

![][3]{: style="max-width:80%;"}

To use alternative custom fonts in your email campaigns, you have the option to create an [email template]({{site.baseurl}}/user_guide/message_building_by_channel/email/templates/email_template/) or [Content Blocks]({{site.baseurl}}/user_guide/message_building_by_channel/email/drag_and_drop/dnd_content_blocks/). However, if you choose to do this, ensure that your font choice is still web-safe and supported on your email platforms. 

### Fallback font

Fallback fonts are used for the title, header, and body text when your default font choice isn't supported by the inbox provider or operating system. By default, Braze automatically sets Arial as a fallback font when global style settings are saved. You also have the option of adding serif or sans serif as options for your default font family.

You can add up to 17 fallback fonts. The first fallback font selected will be the one attempted first. We highly recommend selecting fallback fonts that are similar to your email messaging to maintain consistency across your branding.

## Title styling

Here, you can adjust the styles of your email titles by editing the font size, font color, and text alignment. This applies to the main header and secondary header. 

![Heading styling options for a main header example with the font size set to 34, font color as black, text alignment to center, and text direction as left to right.][6]

Optionally, you can override the default style of your Drag & Drop Editor theme. Click **Override default style** to apply your choice of title styling. This can include setting a different font and link color.

![][7]{: style="max-width:60%;"}

## Text styling

To set a default text style, in the **Text Styling** dropdown, enter the **Font Size** and select **Font Color** to choose a font color from the color picker. 

You can also adjust the block styling for the body text by editing the **Padding Top**, **Padding Right**, **Padding Bottom**, and **Padding Left** values. This will apply to the spacing around all four areas surrounding the text block.

![][4]{: style="max-width:60%;"}

## Button styling

In the **Button Styling** dropdown, you can edit the following default styles for the button:
- Background color
- Font size
- Font color
- Border radius
- Border color
- Border weight
- Button padding

Adjust the block styling for buttons by editing the **Padding Top**, **Padding Right**, **Padding Bottom**, and **Padding Left** values.

![][5]{: style="max-width:50%;"}

After editing the styles in the Drag & Drop Email editor, click **Save**. To further customize your email campaigns and Canvases, check out [Drag & Drop Editor blocks][8]!

[1]: {% image_buster /assets/img_archive/dnd_global_style_settings.png %}
[2]: {% image_buster /assets/img_archive/dnd_basic_styling.png %}
[3]: {% image_buster /assets/img_archive/dnd_custom_font.png %}
[4]: {% image_buster /assets/img_archive/dnd_text_styling.png %}
[5]: {% image_buster /assets/img_archive/dnd_button_styling.png %}
[6]: {% image_buster /assets/img_archive/dnd_heading_styling.png %}
[7]: {% image_buster /assets/img_archive/dnd_default_override.png %}
[8]: {{site.baseurl}}/user_guide/message_building_by_channel/email/drag_and_drop/dnd_editor_blocks