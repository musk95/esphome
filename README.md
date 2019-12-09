# esphome components
## st7789v
  This allows you to play with ST7789V which is TFT-LCD  of ESP32 TTGO.
  
### How to install
  * Copy below folders to the component folder of esphome.<br>
   &nbsp;&nbsp;1. display<br>
   &nbsp;&nbsp;2. image<br>
   &nbsp;&nbsp;3. st7789v<br>

### Samples  
  * Sample - Display image<br>
  ![Cap 2019-12-08 17-31-35-962](https://user-images.githubusercontent.com/11463289/70386805-a7fd8580-19e0-11ea-8ebf-10742a3ab3d1.jpg)<br>

```
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO19

image:
  - file: "ha_esphome.png"
    id: my_image
    type: RGB565

display:
  - platform: st7789v
    id: st7789vdisplay
    reset_pin: GPIO23
    dc_pin: GPIO16
    cs_pin: GPIO5
    bl_pin: GPIO4
    update_interval: 50ms
    lambda: |-
      // Draw the image my_image at position [x=0,y=0]
      it.image(0, 0, id(my_image));
```

  * Sample - Display clock<br>
  ![Cap 2019-12-09 18-24-02-621](https://user-images.githubusercontent.com/11463289/70423607-288bb700-1ab1-11ea-9f83-49684a9fd941.jpg)
<br>
```
spi:
  clk_pin: GPIO18
  mosi_pin: GPIO19
  
font:
  - file: 'slkscr.ttf'
    id: font1
    size: 16
  - file: 'BebasNeue-Regular.ttf'
    id: font2
    size: 85
  - file: 'Arial-Black.ttf'
    id: font3
    size: 20
    
image:
  - file: "ha_esphome.png"
    id: my_image
    type: RGB565
  - file: "clock_bg.png"
    id: clock_bg
    type: RGB565
  
display:
  - platform: st7789v
    id: st7789vdisplay
    reset_pin: GPIO23
    dc_pin: GPIO16
    cs_pin: GPIO5
    bl_pin: GPIO4
    update_interval: 1s
    pages:
      - id: page1
        lambda: |-
          it.image(0, 0, id(my_image));
      - id: page2
        lambda: |-
          it.set_rotation(DISPLAY_ROTATION_90_DEGREES);
          it.image(0, 0, id(clock_bg));
          it.printf(6, 0, id(font1), 0x07e0, "ESPHOME CLOCK");
          // Print time in HH:MM format
          it.strftime(4, 90, id(font2), 0x767E, TextAlign::BASELINE_LEFT, "%H:%M:%S", id(esptime).now());
          it.printf(155, 105, id(font3), 0x3333, "MYTOY");
binary_sensor:
  - platform: gpio
    pin: 
      number: 0
      inverted: True
    name: mode_button
    on_click:
    - min_length: 50ms
      max_length: 350ms
      then:
        - display.page.show: page2
        - component.update: st7789vdisplay

time:
  - platform: homeassistant
    id: esptime
```
