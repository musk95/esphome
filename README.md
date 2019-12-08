# esphome components
## st7789v
  This allows you to play with ST7789V which is TFT-LCD  of ESP32 TTGO.
  
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
