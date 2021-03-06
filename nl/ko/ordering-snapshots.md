---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:codeblock: .codeblock} 
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 스냅샷 주문
{: #orderingsnapshots}

스토리지 볼륨의 스냅샷을 작성하려면 자동으로 또는 수동으로 이를 보유하는 영역을 구매해야 합니다. 용량은 최대 스토리지 볼륨 크기까지 구매가 가능합니다(초기 볼륨 구매 중 또는 여기에 설명된 단계를 사용하여 이후에).

## 주문해야 하는 스냅샷 영역 판별

일반적으로 스냅샷 영역은 다음과 같은 두 가지 주요 요인을 기반으로 스냅샷이 사용합니다.
- 시간 경과에 따라 활성 파일 시스템이 변경된 크기입니다.
- 스냅샷을 유지하려는 기간입니다.  

필요한 영역 크기는 **(변경 비율)** x **(유지되는 시간/일/주/월 데이터 수)**로 계산합니다.

활성 파일 시스템 블록을 표시하는 메타데이터(포인터)의 사본에 불과하므로, 첫 번째 스냅샷은 매우 소량의 영역을 사용합니다.
{:note}

변경사항이 많고 보존 기간이 긴 볼륨은 변경사항이 적당하고 보존 스케줄이 적당한 볼륨에 비해 공간이 많이 필요합니다. 첫 번째 유형의 예는 변경율이 높은 데이터베이스입니다. 두 번째 유형의 예는 VMware 데이터 저장소입니다.

500GB인 실제 데이터의 스냅샷을 12시간마다 작성하고 각 스냅샷 간에 1%가 변경되는 경우, 스냅샷에 60GB의 볼륨이 필요합니다.

*(5GB의 변경 비율) x (12개의 시간별 스냅샷) = (60GB의 사용 영역)*

반대로 실제 데이터가 500GB이고 12시간마다 스냅샷이 작성되며 매시간마다 10%가 변경되는 경우 사용되는 스냅샷 영역은 600GB입니다.

*(50GB의 변경 비율) x (12개의 시간별 스냅샷) = (600GB의 사용 영역)*

따라서 필요한 스냅샷 영역을 판별할 때 변경 비율에 주의해야 합니다. 이는 필요한 스냅샷 영역 변경에 큰 요인으로 사용됩니다. 볼륨이 클수록 더 자주 변경할 가능성이 커집니다. 그러나 변경량이 5GB인 500GB 볼륨과 변경량이 5GB인 10TB 볼륨은 동일한 양의 스냅샷 영역을 사용합니다.

또한, 대부분의 워크로드의 경우 볼륨이 더 커지면 초기 설정 시가 아니라면 설정해야 하는 영역이 줄어듭니다. 기본적으로 기본 데이터 효율성 및 사용자 환경에서 스냅샷이 작동하는 방식 때문에 이와 같이 작동합니다.

## {{site.data.keyword.cloud_notm}} 콘솔을 통해 스냅샷 영역 주문

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/catalog){:new_window}에 로그인하여 왼쪽 상단에 있는 메뉴 아이콘을 클릭하십시오. **클래식 인프라**를 선택하십시오.

   또는 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에 로그인할 수 있습니다.
2. **스토리지** >**{{site.data.keyword.blockstorageshort}}**를 통해 스토리지 LUN에 액세스하십시오.
2. 스냅샷 프레임에서 **스냅샷 영역 변경**을 클릭하십시오.
3. 필요한 영역의 양과 지불 방법을 선택하십시오.
4. **계속**을 클릭하십시오.
5. 사용자에게 있는 임의 **프로모션 코드**를 입력하고 **다시 계산**을 클릭하십시오. 기본적으로 이 주문에 대한 비용 및 주문 검토 필드가 완료됩니다.

   주문을 처리할 때 할인이 적용됩니다.
   {:note}
6. **마스터 서비스 계약을 읽었으며 해당 이용 약관에 동의합니다** 상자를 선택하고 **주문하기**를 클릭하십시오. 스냅샷 영역이 몇 분 내에 프로비저닝됩니다.

## SLCLI를 통해 스냅샷 영역 주문

```
# slcli block snapshot-order --help
사용법: slcli block snapshot-order [OPTIONS] VOLUME_ID

옵션:
  --capacity INTEGER    작성할 스냅샷 영역의 크기(GB) [필수]
  --tier [0.25|2|4|10]  영역을 주문하는 블록 볼륨의 Endurance 스토리지
                        티어(IOPS/GB) [선택적이며 Endurance 스토리지
                        볼륨에 대해서만 유효함]
  --upgrade             주문이 업그레이드임을 나타내기 위한 플래그
  -h, --help            이 메시지를 표시하고 종료합니다.
```
{:codeblock}
