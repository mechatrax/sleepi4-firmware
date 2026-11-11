# sleepi4-firmware  
slee-Pi 4 / slee-Pi 4 Lite を動作させるためのファームウェアを提供します。

## 提供ファイル  
次のファイルがパッケージに含まれています。

### /boot/firmware/overlays/sleepi4.dtbo  
slee-Pi 4 / slee-Pi 4 Lite を動作させるための Device Tree Blob ファイルです。

### /usr/share/doc/sleepi4-firmware/copyright  
パッケージに含まれるファイルの著作権を記述したファイルです。

### /usr/share/doc/sleepi4-firmware/changelog.Debian.gz  
パッケージの変更点を記録したファイルです。

## 設定  
インストール時に次のファイルが変更されます。  
なお、Raspberry Pi 5 に搭載されている RTC は無効になります。  

### /boot/firmware/config.txt  
次のエントリが追記されます。
```
dtoverlay=sleepi4
```

Raspberry Pi 5 では内蔵 RTC と slee-Pi 4 の RTC が競合しないように、[pi5] セクションに次のエントリが追記されます。
```
[pi5]
dtparam=rtc=off
```

Raspberry Pi 5 の内蔵 RTC を使う場合は、この行をコメントアウトしてください。  
パッケージを削除しても dtparam=rtc=off は削除されません。

次のパラメータが指定可能です。
* heartbeat_pin=\<n\>  
  HEARTBEAT 信号の割り当てを変更します。  
  <n> には GPIO 番号を指定します。  

* cpuheartbeat  
  HEARTBEAT 信号を CPU のハードウェアで生成します。  

* interrupt_pin=\<n\>  
  IRQ 信号の割り当てを変更します。  
  <n> には GPIO 番号を指定します。  

* nointerrupt  
  IRQ 信号の設定を無効にします。  

インストール時に次のコマンドが実行されます。

* raspi-config  
  i2c を有効にします。  
  ```
  raspi-config nonint do_i2c 0
  ```
  i2c の周波数を固定します。  
  ```
  raspi-config nonint set_config_var core_freq_fixed 1 /boot/firmware/config.txt
  ```
  USB ポートの電流制限を緩和します（Raspberry Pi 5 用）。  
  ```
  raspi-config nonint set_config_var usb_max_current_enable 1 /boot/firmware/config.txt
  ```
