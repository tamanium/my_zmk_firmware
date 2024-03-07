# zmk firmwareの作成方法について
## フォルダ構成
```
📁my_zmk_firmware
├─ 📄build.yaml
├─ 📄README.md
├─ 📁.github ─ 📁workflows
│               └─ 📄build.yml
└─ 📁config
    ├─ 📄west.yml
    └─ 📁boards ─ 📁shields ─ 📁asym_ble
                                ├─ 📄Kconfig.defconfig
                                ├─ 📄Kconfig.shield
                                ├─ 📄asym_ble.conf
                                ├─ 📄asym_ble.dtsi
                                ├─ 📄asym_ble.keymap
                                ├─ 📄asym_ble.zmk.yml
                                ├─ 📄asym_ble_left.overlay
                                └─ 📄asym_ble_right.overlay
```
## ファイルについて

### 📄build.yaml
```yaml
board: [seeeduino_xiao_ble]
shield: [asym_ble_left, asym_ble_right]
```
### 📄README.md
これ。なくてもOK<br>
### 📄build.yml
```yml
on: [push, pull_request, workflow_dispatch]

jobs:
  build:
    uses: zmkfirmware/zmk/.github/workflows/build-user-config.yml@main
```
### 📄west.yml
```yml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
  self:
    path: config
```
### 📄Kconfig.defconfig
```ini
if SHIELD_LEFT
config ZMK_KEYBOARD_NAME
	default "asym_ble"

config ZMK_SPLIT_ROLE_CENTRAL
	default y

endif
if SHIELD_LEFT || SHIELD_RIGHT

config ZMK_SPLIT
	default y

endif
```
### 📄Kconfig.shield
```ini
config SHIELD_LEFT
	def_bool $(shields_list_contains,asym_ble_left)

config SHIELD_RIGHT
	def_bool $(shields_list_contains,asym_ble_right)
```
### 📄asym_ble.conf
```ini
# CONFIG_ZMK_RGB_UNDERGLOW=y
# CONFIG_WS2812_STRIP=y
# CONFIG_ZMK_USB_LOGGING=y # caused some issues with keys repeating
# CONFIG_ZMK_MOUSE=y
# CONFIG_BT_CTLR_TX_PWR_PLUS_8=y
# CONFIG_GPIO=y
```
### 📄asym_ble.dtsi
かなり長いので省略<br>
### 📄asym_ble.keymap
かなり長いので省略<br>
### 📄asym_ble.zmk.yml
デバイスのメタデータ設定<br>
```yml
file_format: "1"
id: asym_ble
name: asym_ble
type: shield
url: https://github.com/tamanium/my_zmk_firmware/
requires: [seeeduino_xiao_ble]
features:
  - keys
siblings:
  - asym_ble_left
  - asym_ble_right
  
```
### 📄asym_ble_left.overlay
特に各シールドの設定が無ければこのファイルいらない？
```ini
#include "asym_ble.dtsi"
```
### 📄asym_ble_right.overlay
特に各シールドの設定が無ければこのファイルいらない？
```ini
#include "asym_ble.dtsi"

&default_transform {
	col-offset = <6>;
};
```
