---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# 복제 블록 볼륨 작성
{: #duplicatevolume}

기존 {{site.data.keyword.blockstoragefull}}의 복제본을 작성할 수 있습니다. 복제 볼륨은 기본적으로 원래 볼륨의 용량 및 성능 옵션을 상속하며 스냅샷의 특정 시점까지의 데이터 사본을 보유합니다.   

복제는 스냅샷 작성 시점의 데이터를 기반으로 하기 때문에 복제 작성 전에 원본 볼륨에 스냅샷 영역이 있어야 합니다. 스냅샷에 대한 자세한 정보와 스냅샷 영역 주문 방법은 [스냅샷 문서](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)를 참조하십시오.  

복제는 **기본** 및 **복제본** 볼륨 모두에서 작성할 수 있습니다. 원본 볼륨과 동일한 데이터 센터에서 복제가 새로 작성됩니다. 복제본 볼륨에서 복제를 작성하는 경우, 새 볼륨은 복제본 볼륨과 동일한 데이터 센터에서 작성됩니다.

복제 볼륨은 스토리지가 프로비저닝되는 즉시 읽기/쓰기를 위해 호스트에서 액세스 가능합니다. 그러나 원본에서 복제까지 데이터 복사가 완료되지 않으면 스냅샷 및 복제가 허용되지 않습니다.

데이터 복사가 완료되면 복제본은 독립된 볼륨으로 관리 및 사용될 수 있습니다.

이 기능은 대부분의 위치에서 사용할 수 있습니다. 사용 가능한 데이터 센터 목록을 보려면 [여기](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)를 클릭하십시오.

{{site.data.keyword.containerlong}}의 데디케이티드 계정 사용자인 경우에는 [{{site.data.keyword.containerlong_notm}} 문서](/docs/containers?topic=containers-block_storage#block_backup_restore)에서 볼륨 복제에 대한 옵션을 참조하십시오.
{:tip}

복제 볼륨에 대한 일부 공통 사용법:
- **재해 복구 테스트**. 복제를 인터럽트하지 않으면서, 복제본 볼륨의 복제를 작성하여 재해가 발생하는 경우에도 데이터가 온전하며 사용 가능한지 확인합니다.
- **골든 사본**. 다양한 용도로 다중 인스턴스가 작성될 수 있는 골든 사본으로서 스토리지 볼륨을 사용합니다.
- **데이터 새로 고치기**. 테스트 용도로 비프로덕션 환경에 마운트할 프로덕션 데이터의 사본을 작성합니다.
- **스냅샷에서 복원**. 스냅샷 복원 기능으로 전체 원래 볼륨을 겹쳐쓰지 않고 스냅샷의 특정 파일 및 날짜로 원래 볼륨의 데이터를 복원합니다.
- **개발 및 테스트(개발/테스트)**. 한 번에 최대 4개의 동시 볼륨 복제를 작성하여 개발 및 테스트용 복제 데이터를 작성합니다.
- **스토리지 크기 조정**. 데이터를 이동할 필요 없이 크기 또는 IOPS 비율(또는 둘 다)이 새로운 볼륨을 작성합니다.  

몇 가지 방법으로 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 복제 볼륨을 작성할 수 있습니다.


## 스토리지 목록의 특정 볼륨에서 복제 작성

1. {{site.data.keyword.blockstorageshort}} 목록으로 이동하십시오.
    - 고객 포털에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
    - {{site.data.keyword.BluSoftlayer_full}} 콘솔에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 목록에서 볼륨을 선택하고 **조치** > **복제 LUN(볼륨)**을 클릭하십시오.
3. 스냅샷 옵션을 선택하십시오.
    - **비복제본** 볼륨에서 주문하는 경우,
      - **새 스냅샷에서 작성** 선택 - 복제에 사용되는 스냅샷이 작성됩니다. 볼륨에 현재 스냅샷이 없거나 복제본을 즉시 작성하려는 경우 이 옵션을 사용하십시오.<br/>
      - **최신 스냅샷에서 작성** 선택 – 이 볼륨에 대해 존재하는 최신 스냅샷에서 복제가 작성됩니다.
    - **복제본** 볼륨에서 주문하는 경우 스냅샷의 유일한 옵션은 사용 가능한 최신 스냅샷을 사용하는 것입니다.
4. 스토리지 유형 및 위치는 원래 볼륨과 동일하게 유지됩니다.
5. 시간별 또는 월별 비용 청구 - 복제 LUN이 시간별 또는 월별 비용 청구로 프로비저닝되게 선택할 수 있습니다. 원본 볼륨에 대한 비용 청구 유형은 자동으로 선택됩니다. 복제 스토리지에 대해 다른 비용 청구 유형을 선택하려는 경우에는 여기에서 선택할 수 있습니다.
5. 원하는 경우, 새 볼륨에 대해 IOPS 또는 IOPS 티어를 지정할 수 있습니다. 원본 볼륨의 IOPS 지정은 기본적으로 설정됩니다. 사용 가능한 성능 및 크기 조합이 표시됩니다.
    - 원본 볼륨이 0.25 IOPS Endurance티어인 경우, 새로 선택할 수 없습니다.
    - 원본 볼륨이 2, 4 또는 10 IOPR Endurance티어인 경우, 새 볼륨에 대해 해당 티어 사이에서 임의로 이동 가능합니다.
6. 새 볼륨이 원본보다 더 커지도록 해당 크기를 업데이트할 수 있습니다. 원본 볼륨 크기는 기본적으로 설정됩니다.

   {{site.data.keyword.blockstorageshort}}는 원래 볼륨 크기의 10배까지 크기가 조정될 수 있습니다.
   {:tip}
7. 새 볼륨에 대해 스냅샷 영역을 더 추가하거나 더 적게 추가 또는 전혀 추가하지 않도록 업데이트할 수 있습니다. 원본 볼륨의 스냅샷 영역은 기본값으로 설정됩니다.
8. **계속**을 클릭하여 주문을 제출하십시오.

## 특정 스냅샷에서 복제본 작성

1. {{site.data.keyword.blockstorageshort}} 목록으로 이동하십시오.
2. 목록에서 LUN을 클릭하여 세부사항 페이지를 보십시오. (이는 복제본 또는 복제본이 아닌 볼륨일 수 있습니다.)
3. 세부사항 페이지에서 아래로 스크롤하여 목록에서 기존 스냅샷을 선택하고 **조치** > **복제**를 클릭하십시오.   
4. 스토리지 유형(Endurance 또는 Performance) 및 위치는 원본 볼륨과 동일하게 유지됩니다.
5. 사용 가능한 성능 및 크기 조합이 표시됩니다. 원본 볼륨의 IOPs 지정은 기본적으로 설정됩니다. 새 볼륨에 대해 IOPS 또는 IOPS 티어를 지정할 수 있습니다.
    - 원본 볼륨이 0.25 IOPS Endurance티어인 경우, 새로 선택할 수 없습니다.
    - 원본 볼륨이 2, 4 또는 10 IOPS Endurance티어인 경우, 새 볼륨에 대해 해당 티어 사이에서 임의로 이동 가능합니다.
6. 새 볼륨이 원본보다 더 커지도록 해당 크기를 업데이트할 수 있습니다. 원본 볼륨 크기는 기본적으로 설정됩니다.

   {{site.data.keyword.blockstorageshort}}는 원래 볼륨 크기의 10배까지 크기가 조정될 수 있습니다.
   {:tip}
7. 새 볼륨에 대해 스냅샷 영역을 더 추가하거나 더 적게 추가 또는 전혀 추가하지 않도록 업데이트할 수 있습니다. 원본 볼륨의 스냅샷 영역은 기본값으로 설정됩니다.
8. **계속**을 클릭하여 복제에 대한 주문을 제출하십시오.


## SLCLI를 통해 복제 작성
SLCLI에서 다음 명령을 사용하여 복제 {{site.data.keyword.blockstorageshort}} 볼륨을 작성할 수 있습니다.

```
# slcli block volume-duplicate --help
사용법: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

옵션:
  -o, --origin-snapshot-id INTEGER
                                  복제에 사용할 원래 볼륨 스냅샷의 ID.
  -c, --duplicate-size INTEGER    복제 블록 볼륨의 크기(GB). ***크기가
                                  지정되지 않은 경우 원래 볼륨의 크기가
                                  사용됩니다.***
                                  가능한 크기:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] 최소: [원래 볼륨의
                                  크기]
  -i, --duplicate-iops INTEGER    Performance 스토리지 IOPS(100 - 6000 사이의
                                  100의 배수) [Performance 볼륨에만 사용됨]
                                  ***IOPS 값이 지정되지 않는 경우 원래
                                  볼륨의 IOPS 값이 사용됩니다.***
                                  요구사항: [원래 볼륨의 IOPS/GB가 0.3 미만인
                                  경우 복제 볼륨의 IOPS/GB도 0.3 미만이어야
                                  합니다. 원래 볼륨의 IOPS/GB가 0.3 이상인 경우
                                  복제 볼륨의 IOPS/GB도 0.3 이상이어야 합니다.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance 스토리지 티어(IOPS/GB) [Endurance
                                  볼륨에만 사용됨] ***티어가 지정되지 않은 경우
                                  원래 볼륨의 티어가 사용됩니다.***
                                  요구사항: [원래 볼륨의 IOPS/GB가 0.25인 경우
                                  복제 볼륨의 IOPS/GB도 0.25여야 합니다. 원래 볼륨의 IOPS/GB가 0.25보다 큰 경우
                                  복제 볼륨의 IOPS/GB도 0.25보다 커야 합니다.
  -s, --duplicate-snapshot-size INTEGER
                                  복제를 위해 주문할 스냅샷 영역의 크기. *** 스냅샷 영역 크기가 지정되지 않은 경우
                                  원래 블록 볼륨의 스냅샷 영역 크기가
                                  사용됩니다.***
                                  스냅샷 영역이 없는 복제 볼륨을 주문하려면
                                  이 매개변수에 "0"을 입력하십시오.
  --billing [hourly|monthly]      청구 비율에 대한 선택적 매개변수(기본값 monthly)
  -h, --help                      이 메시지를 표시하고 종료합니다.
```
{:codeblock}

## 복제 볼륨 관리

원본 볼륨에서 복제 볼륨으로 데이터가 복사되는 동안 복제가 진행 중임을 나타내는 상태가 세부사항 페이지에 표시됩니다. 이 시간 동안 호스트에 접속하고 볼륨에 읽기/쓰기할 수 있지만 스냅샷 스케줄을 작성할 수는 없습니다. 복제 프로세스가 완료되면 새 볼륨은 소스에서 독립된 볼륨이 되어 정상적으로 스냅샷 및 복제 등으로 관리할 수 있습니다.
