# MSX_PCE2MSX_VGM_CONVERTER
HuC6280(PC Engine / TurboGrafx-16) VGM 파일을 Konami SCC-I(K051649) + MSX PSG(AY‑3‑8910)을 사용하는 
VGM 파일로 변환하는 앱 프로젝트 

이 변환된 파일은 이미 두 칩을 모두 지원하는(Darky와 같은 콤보 카트리지 포함) [vgmplay-msx](https://hg.sr.ht/~grauw/vgmplay-msx)를 통해 실제 MSX 하드웨어(또는 소프트웨어 플레이어)에서 재생할 수 있습니다.  MSX를 위한 실기 또는 에뮬레이터가 아닌 일반적 범용 플레이어등에서는 제대로 플레이되지 않을 수 있습니다. 

## 사전 요구 사항

Windows 10, 11 - 터미널 모드에서 실행하세요 

## 사용법

```
huc6280_msx_converter.exe <input_vgm> <output_vgm>
```

일괄/와일드카드/디렉토리 변환 및 `--scc-vol` / `--psg-vol` 볼륨 옵션에 대해서는 (인수 없이 실행하여) `--help` 출력을 확인하세요.

```
huc6280_msx_converter.exe song.vgz song_scc_psg.vgz
huc6280_msx_converter.exe "Gradius (TG-16)"
huc6280_msx_converter.exe --msxaudio --normalize vgm_collection converted/
```
