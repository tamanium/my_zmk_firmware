
# zmk firmwareの作成方法について
## フォルダ構成
```:フォルダ構成
📁my_zmk_firmware
 ├─ 📄build.yaml
 ├─ 📁.github
 │   └─ 📁workflows
 │       └─ 📄build.yml
 └─ 📁config
     ├─ 📄west.yml
     └─ 📁boards
         └─ 📁shields
             ├─ 📁settings_reset
             └─ 📁asym_ble
                 ├─ 📄Kconfig.defconfig
                 ├─ 📄Kconfig.shield
                 ├─ 📄asym_ble.conf
                 ├─ 📄asym_ble.dtsi
                 ├─ 📄asym_ble.keymap
                 ├─ 📄asym_ble.zmk.yml
                 ├─ 📄asym_ble_left.overlay
                 └─ 📄asym_ble_right.overlay
                       
```

## キーボードレイアウト
```:キーボードレイアウト


Layer Default
┌────┬────┬────┬────┬────┬────┐           ┌────┬────┬────┬────┬────┬────┬────┐
│Eisu│ Q  │ W  │ E  │ R  │ T  │           │ Y  │ U  │ I  │ O  │ P  │ -= │ BS │
├────┴┬───┴┬───┴┬───┴┬───┴┬───┴┐          └┬───┴┬───┴┬───┴┬───┴┬───┴┬───┴────┤
│Caps │ A  │ S  │ D  │ F  │ G  │           │ H  │ J  │ K  │ L  │ ;+ │ Enter  │
├─────┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┐      ┌─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─────┤
│ Shift  │ Z  │ X  │ C  │ V  │ B  │      │ B  │ N  │ M  │ ,< │ .> │ \_ │Shift│
├────────┼────┴────┼────┼────┼────┤      ├────┼────┼────┼────┴────┴────┴─────┘
│ Ctrl   │         │Alt │2 SP│1 SP│      │ 1  │ 2  │ ^~ │
└────────┘         └────┴────┴────┘      └────┴────┴────┘

Layer 1
┌────┬────┬────┬────┬────┬────┐           ┌────┬────┬────┬────┬────┬────┬────┐
│Esc │ 1! │ 2" │ 3# │ 4$ │ 5% │           │ 6& │ 7' │ up │ 8( │ 9) │ 0  │Del │
├────┴┬───┴┬───┴┬───┴┬───┴┬───┴┐          └┬───┴┬───┴┬───┴┬───┴┬───┴┬───┴────┤
│ Tab │ F1 │ F2 │ F3 │ F4 │ F5 │           │Eisu│left│down│rght│ :* │ S+Ent  │
├─────┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┐      ┌─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─────┤
│ Shift  │ F6 │----│----│PrSc│Alt │      │ F2 │PgUp│PgDn│ [{ │ ]} │ /? │Shift│
├────────┼────┴────┼────┼────┼────┤      ├────┼────┼────┼────┴────┴────┴─────┘
│ Ctrl   │         │Alt │2 SP│1 SP│      │ 1  │ 2  │ @` │
└────────┘         └────┴────┴────┘      └────┴────┴────┘

Layer 2
┌────┬────┬────┬────┬────┬────┐           ┌────┬────┬────┬────┬────┬────┬────┐
│Eisu│ !  │ "  │ #  │ $  │ %  │           │ &  │ '  │ up │ (  │ )  │ |  │Del │
├────┴┬───┴┬───┴┬───┴┬───┴┬───┴┐          └┬───┴┬───┴┬───┴┬───┴┬───┴┬───┴────┤
│Caps │ F7 │ F8 │ F9 │F10 │F11 │           │Eisu│left│down│rght│ :* │Alt+Ent │
├─────┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┐      ┌─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─┴──┬─────┤
│ Shift  │F12 │----│----│----│----│      │ F2 │PgUp│PgDn│ {  │ }  │ ?  │Shift│
├────────┼────┴────┼────┼────┼────┤      ├────┼────┼────┼────┴────┴────┴─────┘
│ Ctrl   │         │Alt │2 SP│1 SP│      │ 1  │ 2  │ `  │
└────────┘         └────┴────┴────┘      └────┴────┴────┘



```

## ファイルについて 

### 📄build.yaml
```yaml
#マイコンボード名
board: [seeeduino_xiao_ble]
#シールド名+半角スペース+モジュール名 カンマ区切り
shield: [asym_ble_left rgbled_adapter, asym_ble_right rgbled_adapter, settings_reset] 
```

### 📄build.yml
<details>

<summary>クリックして内容表示</summary>
	
```yml
on: [push, pull_request, workflow_dispatch]

jobs:
  build:
    uses: zmkfirmware/zmk/.github/workflows/build-user-config.yml@main
```
</details>

### 📄west.yml
<details>

<summary>クリックして内容表示</summary>

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
</details>

### 📄Kconfig.defconfig
```yml
# シールド設定で分岐
if SHIELD_LEFT             
# デバイス表示名定数
config ZMK_KEYBOARD_NAME
        # 16文字以内
        default "asym_ble"

# ホストとの接続役かどうか
config ZMK_SPLIT_ROLE_CENTRAL 
        default y          

endif
if SHIELD_LEFT || SHIELD_RIGHT
# 分割キーボードか否か
config ZMK_SPLIT
        default y
endif
```
### 📄Kconfig.shield
シールド設定の定義
```yml
# シールド設定の名称
config SHIELD_LEFT                      #↓第2引数にシールド名
        def_bool $(shields_list_contains,asym_ble_left)
config SHIELD_RIGHT
        def_bool $(shields_list_contains,asym_ble_right)
```
### 📄asym_ble.conf
機能設定
```yml
# アイドル機能 1分
CONFIG_ZMK_IDLE_TIMEOUT = 60000
# ソフトオフ機能 使わない
# CONFIG_ZMK_PM_SOFT_OFF = y
# スリープ機能 使わない
# CONFIG_ZMK_SLEEP=y
```

### 📄asym_ble.dtsi
キー入力の基本設定
```c
#include <dt-bindings/zmk/matrix_transform.h>
/ {

	chosen {
		zmk,battery = &vbatt;
		zmk,matrix_transform = &default_transform;
		zmk,kscan = &kscan0;
	};
	vbatt: vbatt{
		compatible = "zmk,battery-nrf-vddh";
	};
	default_transform: matrix_transform_0 {
		compatible = "zmk,matrix-transform";
		rows = <4>;
		columns = <13>;
		map = <
			RC(0,1)	RC(1,1)	RC(0,2)	RC(0,3)	RC(0,4)	RC(0,5)			RC(0,6)	RC(0,7)	RC(0,8)	RC(0,9)	RC(0,10) RC(0,11) RC(1,11)
			RC(0,0)	RC(2,1)	RC(1,2)	RC(1,3)	RC(1,4)	RC(1,5)			RC(1,6)	RC(1,7)	RC(1,8)	RC(1,9)	RC(1,10) RC(2,11)
			RC(1,0)	RC(3,2)	RC(2,2)	RC(2,3)	RC(2,4)	RC(2,5)		RC(3,6)	RC(2,6)	RC(2,7)	RC(2,8)	RC(2,9)	RC(2,10) RC(3,11)
			RC(2,0)			RC(3,3)	RC(3,4)	RC(3,5)		RC(3,8)	RC(3,9) RC(3,7)
		>;
	};

	kscan0: kscan {
		compatible = "zmk,kscan-gpio-matrix";
		diode-direction = "col2row";
		col-gpios =
			<&xiao_d  0  (GPIO_ACTIVE_HIGH)>,
			<&xiao_d  1  (GPIO_ACTIVE_HIGH)>,
			<&xiao_d  2  (GPIO_ACTIVE_HIGH)>,
			<&xiao_d  3  (GPIO_ACTIVE_HIGH)>,
			<&xiao_d  4  (GPIO_ACTIVE_HIGH)>,
			<&xiao_d  5  (GPIO_ACTIVE_HIGH)>;
		row-gpios =
			<&xiao_d  6  (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>,
			<&xiao_d  7  (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>,
			<&xiao_d  8  (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>,
			<&xiao_d  9  (GPIO_ACTIVE_HIGH | GPIO_PULL_DOWN)>;
	};
};
```


### 📄asym_ble.keymap
キーマップ<br>
```c
/ {
 
//combos...2個以上のキー同時押しによる出力
//使用しないのでコメントアウト
//	combos {
//		compatible = "zmk,combos";
//		combo_q {
//			timeout-ms = <50>;
//			timeout-ms = <50>;
//			key-positions = <0 1>;
//			layers = <0>;
//			bindings = <&kp Q>;
//		};
//	};

//レイヤー設定
//3レイヤー使用予定
	keymap {
		compatible = "zmk,keymap";
		default_layer { bindings = <
				&kp CAPS   &kp Q  &kp W  &kp E  &kp R        &kp T		       &kp Y  &kp U  &kp I	   &kp O       &kp P	    &kp JP_MINUS  &kp BSPC
				&kp CAPS   &kp A  &kp S  &kp D  &kp F        &kp G		       &kp H  &kp J  &kp K	   &kp L       &kp JP_SEMI  &kp ENTER
				&kp LSHFT  &kp Z  &kp X  &kp C  &kp V        &kp B		&kp B  &kp N  &kp M  &kp JP_COMMA  &kp JP_DOT  &kp BSLH	    &kp RSHFT
				&kp LCTRL			&lt 2 SPACE  &lt 1 SPACE	&mo 1  &mo 2
			>;
		};
		layer1 { bindings = <
				&kp ESC  &kp N1  &kp N2  &kp N3  &kp N4     &kp N5	        &kp N6     &kp N7     &kp UP    &kp N8     &kp N9        &kp N0  &kp DEL 
				&kp TAB  &kp F1  &kp F2  &kp F3  &kp F4     &kp F5	        &kp CAPS   &kp LEFT   &kp DOWN  &kp RIGHT  &kp JP_COLON  &kp RS(ENTER)
				&trans   &kp F6  &kp X   &kp C   &kp PSCRN  &kp LALT	&kp F2  &kp CARET  &kp JP_AT  &kp LBKT  &kp RBKT   &kp JP_SLASH  &trans
				&trans				 &trans     &trans	&trans  &trans
			>;
		};
		layer2 {
			bindings = <
				&trans  &kp LS(N1)  &kp LS(N2)  &kp LS(N3)  &kp LS(N4)  &kp LS(N5)	        &kp LS(N6)     &kp LS(N7)     &kp UP        &kp LS(N8)    &kp LS(N9)        &kp LS(N0)  &kp DEL
				&trans  &kp F7      &kp F8      &kp F9      &kp F10     &kp F11		        &trans         &kp LEFT       &kp DOWN      &kp RIGHT     &trans            &kp LA(ENTER)
				&trans  &kp F12	    &trans      &trans      &trans      &trans		&kp F2  &kp LS(CARET)  &kp LS(JP_AT)  &kp LS(LBKT)  &kp LS(RBKT)  &kp LS(JP_SLASH)  &trans
				&trans                                      &trans      &trans          &trans  &trans		
			>;
		};
	};
};

```
### 📄asym_ble.zmk.yml
デバイスのメタデータ設定<br>
```yml
file_format: "1"
id: asym_ble   #キーボード名
name: asym_ble #キーボード名
type: shield
url: https://github.com/tamanium/my_zmk_firmware/
requires: [seeeduino_xiao_ble] #マイコンボード名
features:
  - keys
siblings:
  - asym_ble_left  #左シールド名
  - asym_ble_right #右シールド名
  
```
### 📄asym_ble_left.overlay
左シールド独自の設定を記載(col-gpiosなど)

```c
#include "asym_ble.dtsi"
```
### 📄asym_ble_right.overlay
右シールド独自の設定を記載(col-gpiosやcol番号のオフセットなど)

```c
#include "asym_ble.dtsi"
&default_transform {
	col-offset = <6>;
};
```

