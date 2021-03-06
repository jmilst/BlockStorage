---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 데이터 복제
{: #replication}

복제 중에는 스냅샷 스케줄 중 하나를 사용하여 자동으로 스냅샷을 원격 데이터 센터의 대상 볼륨으로 복사합니다. 사본은 재해가 발생하거나 데이터가 손상된 경우 원격 사이트에서 복구 가능합니다.

복제는 서로 다른 두 위치에 동기화된 데이터를 보관합니다. 볼륨을 복제한 후에 이를 원래 볼륨과 독립적으로 사용하려면 [복제 블록 볼륨 작성](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)을 참조하십시오.
{:tip}

복제하려면 먼저 스냅샷 스케줄을 작성해야 합니다.
{:important}


## 내 복제된 스토리지 볼륨에 대한 원격 데이터 센터 판별

전 세계적으로 {{site.data.keyword.BluSoftlayer_full}}의 데이터 센터는 기본과 원격 조합의 쌍으로 구성됩니다.
데이터 센터 가용성 및 복제 대상에 대한 전체 목록은 표 1을 참조하십시오.

<table>
  <caption style="text-align: left;"><p>표 1 - 이 표는 각 지역의 개선된 기능을 포함한 데이터 센터의 전체 목록을 나타냅니다. 모든 지역에는 별도의 열이 있습니다. 일부 도시(예: 댈러스, 산호세, 워싱턴, 암스테르담, 프랑크푸르트, 런던, 시드니)에는 데이터 센터가 여러 개 있습니다.</p>
  <p>&#42; 미국 1 지역의 데이터 센터에는 개선된 스토리지가 없습니다. 개선된 스토리지 기능이 있는 데이터 센터의 호스트는 미국 1 데이터 센터에서 복제본 대상으로 복제를 시작할 수 <strong>없습니다</strong>.</p>
  </caption>
  <thead>
    <tr>
      <th>미국 1 &#42;</th>
      <th>미국 2</th>
      <th>라틴 아메리카</th>
      <th>캐나다</th>
      <th>유럽</th>
      <th>아시아 태평양</th>
      <th>오스트레일리아</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
TOK04<br />
TOK05<br />
SNG01<br />
SEO01<br />
          CHE01<br />
	        <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
          SYD05<br />
MEL01<br />
          <br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## 초기 복제본 작성

복제는 스냅샷 스케줄을 기반으로 작동합니다. 복제하기 전에 우선 소스 볼륨의 스냅샷 스케줄과 스냅샷 영역이 있어야 합니다. 복제를 설정하려고 하는데 둘 중 하나가 없으면 추가 영역을 구매하거나 스케줄을 설정하도록 프롬프트가 표시됩니다. 복제는 [[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 **스토리지** **{{site.data.keyword.blockstorageshort}}**에서 관리합니다.

1. 스토리지 볼륨을 클릭하십시오.
2. **복제본**을 클릭하고 **복제본 구매**를 클릭하십시오.
3. 복제에서 사용하려는 기존 스냅샷 스케줄을 선택하십시오. 목록에는 모든 활성 스냅샷 스케줄이 포함됩니다. <br />
   시간별, 일별, 주별이 혼합되어 있어도 스케줄은 하나만 선택할 수 있습니다. 이전 복제 주기 이후에 캡처된 모든 스냅샷은 이를 생성한 스케줄에 상관없이 복제됩니다.<br />설정된 스냅샷이 없는 경우, 복제 주문 전에 이를 수행하도록 프롬프트됩니다. 세부사항은 [스냅샷 관련 작업](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)을 참조하십시오.
   {:important}
3. **위치**를 클릭하고 DR 사이트가 되는 데이터 센터를 선택하십시오.
4. **계속**을 클릭하십시오.
5. **프로모션 코드**를 입력(있는 경우)하고 **다시 계산**을 클릭하십시오. 기본적으로 창의 기타 필드가 완료됩니다.

   주문을 처리할 때 할인이 적용됩니다.
   {:note}
6. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오.


## 기존 복제본 편집

[{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}의 **스토리지** **{{site.data.keyword.blockstorageshort}}**에 있는 **기본** 또는 **복제본**에서 복제 스케줄을 편집하고 복제본 영역을 변경할 수 있습니다.


## 복제 스케줄 편집

복제 스케줄은 기존 스냅샷 스케줄을 기반으로 합니다. 복제 스케줄을 변경하려면(예: 시간별에서 일별이나 주별 또는 그 반대로) 복제 볼륨을 취소하고 새로 설정해야 합니다.

하지만 **일별** 복제가 발생할 때 시간을 변경하려면 기본 또는 복제본 탭에서 기존 스케줄을 조정할 수 있습니다.

1. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
2. **스냅샷 스케줄 편집**을 선택하십시오.
3. **스케줄**에서 **스냅샷** 프레임을 확인하여 복제에 사용 중인 스케줄을 판별하십시오. 원하는 스케줄을 변경하십시오.
4. **저장**을 클릭하십시오.


## 복제 영역 변경

기본 스냅샷 영역 및 복제본 영역은 동일해야 합니다. **기본** 또는 **복제본** 탭에서 영역을 변경하는 경우, 소스 및 대상 데이터 센터에 자동으로 영역이 추가됩니다. 스냅샷 영역을 늘리면 즉시 복제 업데이트도 트리거됩니다.

1. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
2. **추가 스냅샷 영역 추가**를 선택하십시오.
3. 목록에서 스토리지 크기를 선택하고 **계속**을 클릭하십시오.
4. **프로모션 코드**를 입력(있는 경우)하고 **다시 계산**을 클릭하십시오. 기본적으로 대화 상자의 기타 필드가 완료됩니다.
5. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오.


## 볼륨 목록에서 복제본 볼륨 보기

**스토리지 > {{site.data.keyword.blockstorageshort}}**의 {{site.data.keyword.blockstorageshort}} 페이지에서 복제 볼륨을 볼 수 있습니다. **LUN 이름**은 뒤에 REP가 표시된 기본 볼륨 이름을 표시합니다. **유형**은 Endurance또는 Performance - 복제본입니다. **대상 주소**는 복제본 볼륨이 복제본 데이터 센터에 마운트되지 않기 때문에 해당 없음이고 **상태**는 비활성을 표시합니다.


## 복제본 데이터 센터에서 복제된 볼륨의 세부사항 보기

**스토리지**, **{{site.data.keyword.blockstorageshort}}**의 **복제본** 탭에서 복제본 볼륨 세부사항을 볼 수 있습니다. 또는, **{{site.data.keyword.blockstorageshort}}** 페이지에서 복제본 볼륨을 선택하고, **복제본** 탭을 클릭해서도 볼 수 있습니다.


## 기본 데이터 센터에서 스냅샷 영역을 늘리면 복제본 데이터 센터에서 스냅샷 영역 증가

볼륨 크기는 기본 및 복제본 스토리지 볼륨에 대해 동일해야 합니다. 크기가 서로 다를 수 없습니다. 기본 볼륨에 대한 스냅샷 영역을 늘리면 복제본 영역도 자동으로 늘어납니다. 스냅샷 영역을 늘리면 즉시 복제 업데이트가 트리거됩니다. 두 볼륨 모두 증가되면 송장의 품목으로 표시되어 필요에 따라 비례 배분됩니다.

스냅샷 영역 늘리기에 대한 자세한 정보는 [스냅샷 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)을 참조하십시오.
{:tip}


## 복제 히스토리 보기

복제 히스토리는 **관리**의 **계정** 탭에 있는 **감사 로그**에서 볼 수 있습니다. 기본 및 복제본 볼륨 모두 동일한 복제 히스토리를 표시합니다. 히스토리에는 다음 항목이 포함되어 있습니다.

- 복제 유형(장애 복구 또는 장애 조치).
- 시작 시기.
- 복제에 사용된 스냅샷.
- 복제의 크기.
- 복제가 완료된 시간.


## 복제본의 복제 작성

기존 {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}}의 복제본을 작성할 수 있습니다. 복제 볼륨은 기본적으로 원래 볼륨의 용량 및 성능 옵션을 상속하며 스냅샷의 특정 시점까지의 데이터 사본을 보유합니다.

복제는 기본 및 복제본 볼륨 모두에서 작성할 수 있습니다. 원본 볼륨과 동일한 데이터 센터에서 복제가 새로 작성됩니다. 복제본 볼륨에서 복제를 작성하는 경우, 새 볼륨은 복제본 볼륨과 동일한 데이터 센터에서 작성됩니다.

복제 볼륨은 스토리지가 프로비저닝되는 즉시 읽기/쓰기를 위해 호스트에서 액세스 가능합니다. 그러나 원본에서 복제까지 데이터 복사가 완료되지 않으면 스냅샷 및 복제가 허용되지 않습니다.

자세한 정보는 [복제 블록 볼륨 작성](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)을 참조하십시오.

## 재해 발생 시 복제본을 사용하여 장애 복구

장애 복구 시에, 기본 데이터 센터의 스토리지 볼륨에서 원격 데이터 센터의 대상 볼륨으로 "스위치를 돌립니다". 예를 들어, 기본 데이터 센터가 런던이고 보조 데이터 센터는 암스테르담입니다. 장애가 발생하는 경우 암스테르담의 컴퓨팅 인스턴스에서 현재 기본인 볼륨으로 연결하여 암스테르담으로 장애 복구합니다. 런던의 볼륨이 복구되면 런던 및 런던의 컴퓨팅 인스턴스의 기본 볼륨으로 장애 조치할 수 있도록 암스테르담 볼륨의 스냅샷이 작성됩니다.

* 1차 위치에 위험이 임박하거나 심각하게 충격을 받은 경우 [액세스 가능한 1차 볼륨으로 장애 복구](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible)를 참조하십시오.
* 1차 위치가 작동 중지된 경우 [액세스할 수 없는 1차 볼륨으로 장애 복구](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible)를 참조하십시오.


## 기존 복제본 취소

복제는 즉시 수행되거나 매년 지정일에 취소할 수 있으며 비용 청구를 종료합니다. 복제는 **기본** 또는 **복제본** 탭에서 취소할 수 있습니다.

1. **{{site.data.keyword.blockstorageshort}}** 페이지에서 볼륨을 클릭하십시오.
2. **기본** 또는 **복제본** 탭에서 **조치**를 클릭하십시오.
3. **복제본 취소**를 선택하십시오.
4. 취소 시기를 선택하십시오. **즉시** 또는 **매년 지정일**을 선택하고 **계속**을 클릭하십시오.
5. **취소로 인해 데이터 유실이 발생할 수 있음을 인지합니다.**를 클릭하고 **복제본 취소**를 클릭하십시오.


## 기본 볼륨이 취소된 경우 복제 취소

기본 볼륨이 취소되면 복제 스케줄 및 복제본 데이터 센터의 볼륨은 삭제됩니다. 복제본은 {{site.data.keyword.blockstorageshort}} 페이지에서 취소됩니다.

 1. **{{site.data.keyword.blockstorageshort}}** 페이지에서 볼륨을 강조표시하십시오.
 2. **조치**에서 **{{site.data.keyword.blockstorageshort}} 취소**를 선택하십시오.
 3. 취소 시기를 선택하십시오. **즉시** 또는 **매년 지정일**을 선택하고 **계속**을 클릭하십시오.
 4. **취소로 인해 데이터 유실이 발생할 수 있음을 인지합니다.**를 클릭하고 **취소**를 클릭하십시오.

## SLCLI의 복제 관련 명령
{: #clicommands}

* 특정 볼륨에 적합한 복제 데이터 센터를 나열합니다.
  ```
  # slcli block replica-locations --help
  사용법: slcli block replica-locations [OPTIONS] VOLUME_ID

  옵션:
  --sortby TEXT   열 정렬 기준
  --columns TEXT  열 표시. 옵션: ID, Long Name, Short Name
  -h, --help      이 메시지를 표시하고 종료합니다.
  ```

* 블록 스토리지 복제본 볼륨을 주문합니다.
  ```
  # slcli block replica-order --help
  사용법: slcli block replica-order [OPTIONS] VOLUME_ID

  옵션:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  복제에 사용할 스냅샷 스케줄
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [필수]
  -l, --location TEXT             복제를 위한 데이터 센터의 단축 이름
                                  (예: dal09)  [필수]
  --tier [0.25|2|4|10]            복제를 주문하는 기본 볼륨의 Endurance
                                  스토리지 티어(IOPS/GB)
                                  [선택적]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  복제를 주문하는 기본 볼륨의 운영 체제
                                  유형(예: LINUX)
                                  [선택적]
  -h, --help                      이 메시지를 표시하고 종료합니다.
  ```

* 블록 볼륨에 대한 기존의 복제 볼륨을 나열합니다.
  ```
  # slcli block replica-partners --help
  사용법: slcli block replica-partners [OPTIONS] VOLUME_ID

  옵션:
  --sortby TEXT   열 정렬 기준
  --columns TEXT  열 표시. 옵션: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      이 메시지를 표시하고 종료합니다.
  ```

* 특정 복제 볼륨에 블록 볼륨의 장애 복구를 수행합니다.
  ```
  # slcli block replica-failover --help
  사용법: slcli block replica-failover [OPTIONS] VOLUME_ID

  옵션:
  --replicant-id TEXT  복제 볼륨의 ID
  --immediate          복제 볼륨으로 즉시 장애 복구를 수행합니다.
  -h, --help           이 메시지를 표시하고 종료합니다.
  ```

* 특정 복제 볼륨에서 블록 볼륨의 장애 조치를 수행합니다.
  ```
  # slcli block replica-failback --help
  사용법: slcli block replica-failback [OPTIONS] VOLUME_ID

  옵션:
  --replicant-id TEXT  복제 볼륨의 ID
  -h, --help           이 메시지를 표시하고 종료합니다.
  ```
