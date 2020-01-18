# Intel Xeon E3-26xx Unlock TurboBoost via EFI Shell

## Подготовка ESP c Windows
Чтобы не иметь мучений с созданием загрузочной флешки или программатором, можно скопировать Shellx64.efi в корень ESP (загрузочной партиции в режиме EFI), для этого ее надо подключить командой в черном окне от администратора, например,
```
C:\Windows\system32>cd \Users\Tester\Desktop\BIOS
C:\Users\Tester\Desktop\BIOS> mountvol x: /S
C:\Users\Tester\Desktop\BIOS> copy “3.Driver V3.EFI\V3.efi” x:\EFI\BOOT
C:\Users\Tester\Desktop\BIOS> copy “3.Driver V3.EFI\Shellx64.efi” x:\
```
Прошить биос можно также из окружения EFI, скачав прошивалку с официального сайта AMI - https://ami.com/en/?Aptio_V_AMI_Firmware_Update_Utility.zip, И разаривировать из него AfuEfi64.zip, а из него собственно прошивалку AfuEfix64.efi

Чтобы прошить, необходимо скопировать аналогичным образом новый образ биоса и саму прошивалку на ESP партицию
```
C:\Users\Tester\Desktop\BIOS> copy “1.FPTBIOS\backup.bin” x:\
C:\Users\Tester\Desktop\BIOS> copy “AfuEfix64.efi” x:\
```
## Загрузка в EFI Shell и прошика обновленного биоса
Далее при перезагрузке компа, в меню  Boot  выбрать пункт поиска выполняемого шелла на носителе - Launch EFI Shell from filesystem device, никаких флешек можно не вставлять, 
Так как выше был скопирован шелл в корень загрузочной партиции, то именно его и найдет и запустит биос. Далее, необходимо прошить биос и добавить загрузку EFI драйвера, как описано в видео, а именно, 
```
Shell> map fs*
# покажет все найденные файловые системы, надо найти ту, на которую скопировали файлы, например
Shell> dir fs0:
# покажет файлы  Shellx64.efi, bios.bin, AfuEfix64.efi
# Добавляем драйвер V3.efi в процесс инициализации
Shell> fs0:
FS0:\> bcfg driver add 0 \EFI\BOOT\V3.efi “E5-26xx unlock TurboBoost”
# Прошиваем обновленный биос
FS0:\> AfuEfix64.efi bios.bin /P /B
# Перегружаем комп
FS0:\> reset
```

## Заключение
Да, этот метод работает при прошивке в том числе и на плату X99z v102, ты спрашивал в другом видео (https://www.youtube.com/watch?v=RmI0T5Vp46E) как избежать какой-то ошибки при прошивке биоса из-под виндоуса через fpt
