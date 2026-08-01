# <img width="64" height="64" alt="icon@2x" src="https://github.com/user-attachments/assets/29025370-fa25-41a1-ad18-5ca567dbf120" />  홈어시스턴트 BLE ADV 천장 선풍기 / 조명

[![GitHub release](https://img.shields.io/github/v/release/woosun/ha-ble-adv_kr.svg)](https://github.com/woosun/ha-ble-adv_kr/releases/)
![Usage](https://img.shields.io/badge/dynamic/json?color=9932CC&logo=home-assistant&label=usage&suffix=%20installs&cacheSeconds=15600&url=https://analytics.home-assistant.io/custom_integrations.json&query=$.ble_adv.total)

> **한국어 포크**: 이 저장소는 [NicoIIT/ha-ble-adv](https://github.com/NicoIIT/ha-ble-adv)의 한국어 포크입니다. 통합 구성 UI 전체가 한국어로 번역되었습니다.

**_BLE Raw Advertising_** 방식으로 다양한 브랜드의 천장 선풍기 / 조명 장치를 제어하는 홈어시스턴트 커스텀 통합입니다.

이 통합은 **특정 기기 유형이나 브랜드에 국한되지 않습니다**: 다양한 [폰 앱](#지원-천장-선풍기--조명-프로토콜)과 리모컨이 사용하는 통신 프로토콜을 인식하고 재현할 수 있습니다.

## 주요 기능
* 이미 페어링된 컨트롤러(안드로이드 폰 앱, 물리적 리모컨)의 명령을 수신하여 기기 설정을 자동으로 감지
* 기존 홈어시스턴트 UI를 사용하여 홈어시스턴트 선풍기/조명 엔티티 생성
* 폰 앱에서 전송한 명령을 수신하고 홈어시스턴트 엔티티 상태 업데이트
* 다른 컨트롤러 동기화: 폰 앱과 리모컨이 모두 홈어시스턴트 엔티티 상태를 업데이트할 수 있도록 지원
* 홈어시스턴트 UI 설정 흐름을 기반으로 한 완전한 안내형 구성
* 홈어시스턴트 호스트의 블루투스를 사용하거나 ESPHome 기반 [ble_adv_proxy](https://github.com/NicoIIT/esphome-ble_adv_proxy) 사용 가능 (ESPHome `bluetooth_proxy`와 유사하나 _BLE Raw Advertising_ 지원)

## 요구 사항
* 홈어시스턴트가 _다음 중 하나_ 를 충족해야 합니다:
  * LINUX 호스트에서 표준 HA 설치(HAOS)를 사용하고 호스트에 직접 접근 가능한 블루투스 어댑터(내장 또는 USB, VM 레이어 사용 안 함)가 있어야 합니다. [블루투스 통합](https://www.home-assistant.io/integrations/bluetooth/)에서 인식되는 것이 좋습니다(필수는 아님). 자체 HA 도커 컨테이너(루트가 아닌 또는 'host' 네트워크가 아닌)를 정의한 **고급** 사용자의 경우 [여기](https://github.com/NicoIIT/ha-ble-adv/wiki/Workaround-for-HA-non-'network_mode:-host'-or-non-root-installations)에 해결 방법이 있습니다.
  * 홈어시스턴트 인스턴스에 하나 이상의 ESPHome [ble_adv_proxy](https://github.com/NicoIIT/esphome-ble_adv_proxy)가 연결되어 있어야 합니다. 이미 [bluetooth_proxy](https://esphome.io/components/bluetooth_proxy/)가 있다면 `ble_adv_proxy`로 쉽게 확장할 수 있습니다.
* 기기가 홈어시스턴트 호스트 또는 `ble_adv_proxy`의 블루투스 범위 내에 있어야 합니다.
* 최신 홈어시스턴트 코어(최소 2025.2.4)와 HACS(최소 2.0.1)가 필요합니다.

## 지원 천장 선풍기 / 조명 프로토콜
지원되는 프로토콜은 다음 안드로이드 폰 앱(대부분 iOS도 지원)에서 사용하는 것입니다:

* [LampSmart Pro](https://play.google.com/store/apps/details?id=com.jingyuan.lamp)
* [FanLamp Pro](https://play.google.com/store/apps/details?id=com.jingyuan.fan_lamp)
* [ApplianceSmart](https://play.google.com/store/apps/details?id=com.jingyuan.smart_home)
* [Vmax smart](https://play.google.com/store/apps/details?id=com.jingyuan.vmax_smart)
* [Zhi Jia](https://play.google.com/store/apps/details?id=com.cxw.cxwblelight)
* [Zhi Guang](https://play.google.com/store/apps/details?id=com.cxw.zhiguang)
* [Zhi Mei Deng Kong](http://mihuan.iotworkshop.com/zhiguang/) (Play Store에 없음)
* [Mantra Lighting](https://play.google.com/store/apps/details?id=com.newenergy.baolilan)
* [Smart Light / Argrace Smart](https://apkpure.com/argrace-smart/ai.argrace.oem) (RGB 미지원, 기기별 제어만 지원, 마스터 제어 미지원) (Play Store에서 제거됨, 중단된 것으로 보임)
* [LE Light Pro / 乐智光Pro](https://openapi.lelight.top/dl/cqan) (Play Store에 없음)
* [RuiXin](https://rx-etech.com/rxzn.html) Sanweyter 기기 (RGB 미지원, 타이머 미지원, 리모컨 온도 스위치가 HA에 반영 안 됨) (Play Store에 없음)
* [RW.LIGHT](https://play.google.com/store/apps/details?id=com.rw.rwblelight) (그룹 제어 미지원, AURA / IR / FIBER 유형 미지원)
* [Smart Elfin](https://play.google.com/store/apps/details?id=com.warpfuture.wfiot.g)
* [GMIMA](https://www.jasonghost.com/lampSmartGmima/) (Play Store에 없음)
* 기타 (레거시), Play Store에서 제거된 앱: 'FanLamp', 'ControlSwitch', 'Lamp Smart Pro - Soft Lighting / Smart Lighting'

사용 중인 앱의 프로토콜이 아직 지원되지 않는 경우 [여기](https://github.com/NicoIIT/ha-ble-adv/issues/new?template=new_app.yml)에서 지원을 요청할 수 있습니다.

## 지원 물리적 리모컨
다양한 물리적 리모컨이 존재하므로 테스트 없이 특정 리모컨이 지원되는지 알 수 없습니다. 일부는 폰 앱과 동일한 프로토콜을 사용하고, 다른 일부는 자체 프로토콜, RF 또는 IR을 사용합니다.

리모컨이 통합에서 인식되지 않는 경우 지원 가능 여부를 확인하고 지원을 요청할 수 있습니다. [방법 보기](https://github.com/NicoIIT/ha-ble-adv/wiki/How-to-know-if-my-Physical-Remote-is-using-BLE-Advertising-to-control-my-device).

## 통합 설치
[HACS (Home Assistant Community Store)](https://www.hacs.xyz/)를 통해 직접 다운로드 및 설치하세요. 다음 [my-link](https://my.home-assistant.io/) 링크를 사용합니다:

[![홈어시스턴트 인스턴스를 열고 홈어시스턴트 커뮤니티 스토어에서 저장소를 추가합니다.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=woosun&repository=ha-ble-adv_kr)

저장소를 추가한 후 반영되도록 홈어시스턴트를 재시작해야 합니다.

## 통합 추가
통합이 설치되면 제어하려는 각 기기마다 **"BLE ADV 천장 선풍기 / 조명"** 통합을 [추가](https://www.home-assistant.io/getting-started/integration/)할 수 있습니다.

구성 흐름은 폰/물리적 리모컨에서 전송된 명령을 수신하고 기기를 제어(조명 깜빡임)하려고 시도하므로, 다음을 확인하기 위해 **기기와 같은 방에 있어야 합니다**:
* 블루투스 어댑터가 사용 위치에서 명령을 수신할 수 있는지
* 동일한 블루투스 어댑터가 기기를 제어할 수 있는지
* 조명이 깜빡였는지 확인할 수 있는지

구성 흐름의 주요 단계는 다음과 같습니다:
* **구성 감지**, 2가지 방법:
  * **권장 방법 - 설정 복사**: 이미 페어링되어 기기를 제어하는 폰 앱(또는 물리적 리모컨)에서 버튼을 누르면, 구성 과정이 가능한 설정을 자동으로 감지합니다.
  * **페어링**: 기기를 제어하는 이미 페어링된 컨트롤러가 없는 경우, 기기와 페어링을 시도합니다.
* **검증**: 조명을 깜빡이게 해서 감지/입력된 잠재 설정 중 기기를 제어할 수 있는 첫 번째 설정을 찾습니다.
* **정의**: 생성할 **엔티티**(메인 조명, 두 번째 조명, 선풍기 등)와 특성(RGB / 냉난색 화이트 / on-off / 선풍기 속도 / 최소 밝기 등)을 정의하고, 추가 리모컨 컨트롤러를 추가하거나 기술 매개변수를 수정합니다. 이 단계는 나중에 통합을 재구성하여 수정할 수 있습니다 ([Wiki](https://github.com/NicoIIT/ha-ble-adv/wiki/Configuration-Guide) 참조).
* **완료**: 기기 이름을 지정하고 변경 사항을 저장합니다.


## 향후 개발
향후 개발은 [GitHub 기능 요청](https://github.com/NicoIIT/ha-ble-adv/issues?q=is%3Aissue%20state%3Aopen%20label%3Aenhancement)에서 추적됩니다. 필요한 기능에 👍를 주어 투표하거나 새 요청을 열어주세요!

## Wiki에서 더 보기
* [구성 가이드](https://github.com/NicoIIT/ha-ble-adv/wiki/Configuration-Guide)
* [문제 해결 가이드](https://github.com/NicoIIT/ha-ble-adv/wiki/Troubleshooting-Guide)
