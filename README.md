
[![hacs_badge](https://img.shields.io/badge/HACS-Integration-41BDF5.svg)](https://github.com/hacs/integration)
[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg

# Add your custom icons to Home Assistant!

Based on the amazing work from [Custom Brand Icons](https://github.com/elax46/custom-brand-icons) by @elax46 

With this repository, you will be able to add custom icons from your own icon set into Home Assistant and use them on the Lovelace dashboard:

![Custom icons](https://raw.githubusercontent.com/mathoudebine/homeassistant-custom-icons/main/custom-icons.png)

## Pre-requisites & limitations

Mandatory:
* you must have Python 3 installed on your computer
* your icons must be in SVG format
* the `svg` node must contain a `viewBox` property only. Any other property like `transform`, `translate`, or `scale` will be ignored
* the `svg` must contain one or several of the following elements: `path`, `circle`, `ellipse`, `polygon`, `polyline` or `rect`
* shapes can be inside a `g` node, but any properties from the `g` node are ignored

Recommended:
* your icons are square: the viewBox property will be something like `viewBox="0 0 128 128"`
* your icons are centered horizontally & vertically inside the viewBox
* your icons contain only one SVG element (ideally one `path`) OR only elements of the same type: you can use [this online converter to do this](https://thednp.github.io/svg-path-commander/convert.html)

Limitations:
* `style` or `fill` properties are ignored: icons can be monochrome only. Home Assistant will manage icons color based on theme and entity state.

To make or edit an icon in svg format you can use different programs starting from illustrator, inkview, or [Inkscape](https://inkscape.org/).

## How to use

### 1. Download or fork this repository

If you fork this repository it will be public on Github and cannot be changed. 
If you don't want to share your icons publicly, download this repository instead of forking.

### 2. Add your SVG icons

Add all your SVG icons to the `icon-svg` folder. The name of the file will be the icon name in Home Assistant.

You can remove SVG files already present for demo purposes.

### 3. Generate icons .js file for Home Assistant

Use `python svg-to-js.py` to convert your SVG icons to Home Assistant format. This script will create a `custom-icons.js` file containing all your icons.

See script output if some icons have not been processed. Fix the SVG the relaunch the script.

### 4. Add your icons to Home Assistant

#### Using HACS

This method is only available if you forked this repository, or if you copied its content to another Github **public** repository.

1. Commit the generated `custom-icons.js` file to your repository main branch
2. Make sure HACS is installed.
3. Go to HACS > Frontend > Three dots > Custom repositories.
4. Add your Github repository URL as a custom repository (category: lovelace).
5. Install "My Custom Icons" that appeared in your Interface tab. You can customize the name by editing `hacs.json`.

After installing through HACS:
1. Add the following lines to your `configuration.yaml`:

    ```yaml
    frontend:
      extra_module_url:
        - /local/community/homeassistant-custom-icons/custom-icons.js
    ```
   (Replace `homeassistant-custom-icons` by your Github repository name if you changed it)

2. **Restart Home Assistant.**

3. Hard Reload the homepage from your browser by holding CTRL and pressing F5. For Mac, hold ⌘ CMD and ⇧ SHIFT, then press R.

#### Manual Installation

1. Copy the generated `custom-icons.js` file into `<config>/www/` folder. `<config>` is the directory where your `configuration.yaml` resides.

2. Add the following lines to your `configuration.yaml`:

    ```yaml
    frontend:
      extra_module_url:
        - /local/custom-icons.js
    ```

3. (Optional) YAML mode users: Go to Home Assistant Settings > Dashboards > Three dots > Resources > Add Resource  
   * URL: `/local/custom-icons.js`  
   * Resource Type: JavaScript module  

4. **Restart Home Assistant.**

5. Hard Reload the homepage from your browser by holding CTRL and pressing F5. For Mac, hold ⌘ CMD and ⇧ SHIFT, then press R.

### 5. Use your custom icons

Custom icons are available in all Home Assistant under the prefix `custom:`, as opposed to Home Assistant icons that use the prefix `hass:` or `mdi:`
- Example: `custom:my_icon1` for an icon that was named `my_icon1.svg`

---
## Troubleshooting

### I can't find my new icons in Home Assistant

#### Hard Reload (browser cache issue)
- Reload browser by holding CTRL and pressing F5.
- For Mac, hold ⌘ CMD and ⇧ SHIFT, then press R.

#### Redownload Integration
1. From left sidebar, select on *HACS*.
2. Select on *Integrations*.
3. From the top header bar (Integrations, Frontend), select *Frontend*.
4. Search *My Custom Icons* on the search bar.
5. Select *My Custom Icons*.
6. From the top right, select the 3 vertical dots which opens a dropdown menu.
7. Select *Redownload*.
8. **Hard reload** browser.

#### Reinstall Integration
1. Open the dropdown menu from **Step 6** of **Redownload Integration**.
2. Select *Remove*, then select *Remove* again on the popup.
3. This should bring you back to /hacs/frontend
4. From the top right, select the 3 vertical dots which opens a dropdown menu.
5. Select on *Custom repositories*.
6. Find *Custom brand icons* and select it.
7. On the bottom right, select the big blue *Download* icon.
8. **Hard reload** browser.

### My custom icons don't look like the original SVG in Home Assistant

Some icons may look different in Home Assistant. This is because the SVG content needs to be adapted to the Home Assistant format.  
The Home Assistant format accept a unique `path` element, so every other SVG elements like `circle`, `rect` ... must be converted to the `path` format then concatenated. This may lead to some visual differences.  
This can also be caused by an SVG with multiple content. The superposition might be different after elements are converted and concatenated to a unique `path`.

Try to rework your SVG to use only `path` elements if possible.
You can convert your SVG using tools like https://thednp.github.io/svg-path-commander/convert.html or https://github.com/elrumordelaluz/path-that-svg