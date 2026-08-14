# MSX_PCE2MSX_VGM_CONVERTER
HuC6280(PC Engine / TurboGrafx-16) VGM 파일을  
Konami SCC-I(K051649) + MSX PSG(AY‑3‑8910)을 사용하는 VGM 파일로 변환하는 앱 프로젝트 

---- What is "MSX_PCE2MSX_VGM_CONVERTER"  
https://youtu.be/OHTTWZLqZQM?si=htLMSQyLQPws04d0  

이 변환된 파일은 MSX의 SCCI와 MSX-AUDIO 커트리지와 함께 [vgmplay-msx](https://hg.sr.ht/~grauw/vgmplay-msx)를 통해   
실제 MSX 하드웨어(또는 openMsx, blueMSX등 에뮬레이터)에서 재생할 수 있습니다.    
MSX를 위한 실기 또는 에뮬레이터가 아닌 일반적 범용 플레이어등에서는 제대로 플레이되지 않을 수 있습니다. 

## 사전 요구 사항

Windows 10, 11 - 터미널 모드에서 실행하세요   
이미 알려진 바와 같이 MSX측의 메모리는 vgmplay가 플레이할 vgm파일보다 큰 사이즈를 요구합니다. 

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


## 1. 기본 사용법

```
huc6280_msx_converter.exe <input.vgm> <output.vgm>       단일 파일 변환
huc6280_msx_converter.exe <pattern...>                   일괄 변환 (와일드카드 * ? 가능)
                                                         출력 파일명: <원본이름><접미사>.vgm
                                                         (접미사는 DDA 타깃에 따라 다름 - 아래
                                                         "DDA(PCM) 출력 타깃" 참고. 기본값은
                                                         _scc_psg_2610b)
huc6280_msx_converter.exe <directory>                    디렉터리 안의 모든 .vgm/.vgz를 재귀적으로 변환
huc6280_msx_converter.exe <inputs...> <output_dir>       여러 입력을 output_dir 폴더로 일괄 변환
                                                         (하위 폴더 구조 그대로 유지)
```

`.vgz`(gzip 압축 VGM)는 입력/출력 모두 지원됩니다. 입력 VGM은 **HuC6280 레지스터
쓰기와 지연/종료 커맨드만 있는, HuC6280 단독 칩 리핑**이어야 합니다(PC Engine vgm 리핑은
보통 이 형태입니다).


### DDA(PCM) 출력 타깃 옵션 - 아래 중 최대 1개만 선택, 기본값은 Neotron (YM2610/b)

| 옵션 | 설명 |
|---|---|
| (지정 안 함)      | **Neotron(YM2610/B) ADPCM-B (기본값)**. 실제 야마하 ADPCM-B로 인코딩해 Neotron 카트리지가 자체적으로 재생. Neotron 카트리지가 있어야 DDA가 연주되며, SCC+PSG만 있는 기기에서는 이 파일의 DDA 부분만 무음(멜로디는 정상 재생). |
| `--ym2608`      | Makoto(YM2608) ADPCM-B로 대체. 동일한 방식, 다만 샘플 RAM이 256KB로 고정돼 있어 PCM 비중이 아주 큰 곡은 다운샘플이 더 걸릴 수 있음. |
| `--msxaudio   ` | Y8950(MSX-AUDIO). `--ym2608`과 같은 256KB 샘플 RAM 필수.  - MSX-AUDIO(Y8950) 유닛이 있어야 DDA가 연주됨 - SCC+PSG만 있는 기기에서는 Neotron/Makoto와 동일하게 DDA 부분만 무음. |

### 기타 옵션

| 옵션 | 설명 |
|---|---|
| `--scc-vol N        `  | SCC(-I) 볼륨 오프셋. 0-15 볼륨 레지스터 값에 그대로 더해짐(음수 가능). 기본값 0 |
| `--psg-vol N        `  | PSG(AY-3-8910) 볼륨 오프셋. 위와 동일 단위. **기본값 -3**(v18부터 - 레지스터 값 계산 자체엔 편향이 없지만 실제 재생 시 PSG가 SCC보다 크게 들려서 사용자가 0/-3/-5/-7 중 직접 골라 확정한 값임 |
| `--adpcmb-vol N  ` | 기본 Neotron/`--ym2608`/`--msxaudio` 타깃에서 볼륨 조절. ADPCM-B 출력 레벨 레지스터 원값(0-255, 선형). **기본값 85(0x55)** |
| `--normalize` | 기본 Neotron/`--ym2608`/`--msxaudio` 타깃에서 DDA를 제외한 모든 멜로디 채널(SCC/PSG)의 볼륨을 그 파일 자신의 최대치 기준으로 평준화(파일별로 배율 자동 계산, 인자 없는 on/off 플래그). `--normalize`를 쓰면 배치 변환 출력 파일명 끝에 `_N`이 붙음. vgm파일 리핑 타입에 따라 볼륨이 들쭉날쭉한 문제를 이 옵션으로 평준화 |

---

