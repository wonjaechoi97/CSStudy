# Memory Management

## Multilevel Paging and Performance
>- Address space가 더 커지면 다단계 페이지 테이블이 필요하다.
>- 각 단계의 페이지 테이블이 메모리에 존재하므로 logical address의 physical address 변환에 더 많은 메모리 접근이 필요하다.
>- TLB를 통해 메모리 접근 시간을 줄일 수 있다.
>- 4단계 페이지 테이블을 사용하는 경우
>   - 메모리 접근 시간이 100ns, TLB 접근 시간이 20ns이고
>   - TLB를 거쳐 메모리를 찾을 ratio가 98%인 경우 
>     - effective memory access time = 0.98 x 120 + 0.02 x 520 = 128 nanoseconds.
>     - 결과적으로 주소변환을 위해 28ns만 소요한다.

## Valid(v) Invalid(i) Bit int a Page Table
>- page table entry에는 사용되지 않는 영역을 위해서도 entry가 만들어져야 한다.
>- valid/invalid bit을 통해서 entry의 사용 여부를 표시한다.

## Memory Protection
> ### Page table의 각 entry마다 아래의 bit를 둔다
>#### Protection bit
>>page에 대한 접근 권한
>>어떤 연산에 대한 접근 권한을 나타낸다.(write/reade/read-only)
>#### Valid-invalid bit
>> "valid"는 해당 주소의 frame에 그 프로세스를 구성하는 유효한 내용이 있음을 뜻한다.(접근 허용)
>> "invalid"는 해당 주소의 frame에 유효한 내용이 없음을 뚯한다.(접근 불허)

## Inverted Page Table 
> ### page table이 큰 이유
>> 모든 process 별로 그 logical address에 대응하는 모든page에 대해 page table entry가 존재한다.
>> 
>> 대응하는 page가 메모리에 있든 아니든 간에 page table에는 entry로 존재한다.
>> 
>### inverted page table
> 
>> page frame 하나당 page table에 하나의 entry를 둔 것이다.
>> 
>> 각 page table entry는 가가의 물리적 메모리의 page frame이 담고 있는 내용을 표시한다.(process id, process의 logical address)
>> 
>>  #### 단점 
>>-  테이블 전체를 탐색해야 한다.
>>- 순차적으로 전체를 탐색하면 너무 오래 걸린다.
>>
>>  #### 조치
>>-  table을 associative register라는 별도의 하드웨어에 넣어서 사용한다(expensive).

## Shared pages
> 같은 프로그램을 사용하면 일부 페이지를 공유할 수 있다.
> 
> ### shared code
>  
>> Re-entrant Code (=Pure code)
>> 
>> read-only로 하여 프로세스 간에 하나의 code만 메모리에 올린다.
>> 
>> shared code는 모든 프로세스의 logical address space에서 동일한 위치에 있어야 한다.
>
> ### private code and data
>> 각 프로세스들은 독자적으로 메모리에 올린다.
>>
>>  Private data는 logical address space의 아무 곳에 와도 무방하다.

## Segmentation
> ### 프로그램은 의미 단위인 여러 개의 segment로 구성된다.
>> 작게는 프로그램을 구성하는 함수 하나하나를 세그먼트로 정의한다. 
>> 
>> 크게는 프로그램 전체를 하나의 세그먼트로 정의 가능하다.
>> 
>> 일반적으로는 code, data, stack 부분이 하나씩의 세그먼트로 정의된다. 
>> 
> ### Segment는 다음과 같은 logical unit들이다.
>> - main()
>> - function
>> - global variables
>> - stack
>> - symbol table, arrays


## Segmentation Architecture
> ### Logical address는 다음의 두 가지로 구성된다.
>> < segment-number, offset >
> ### Segmnet table
>> 각 테이블의 엔트리에는
>>> - base:segment가 시작하는 physical address
>>> 
>>> - limit: segment의 길이
>>> 
> ### segment-table base register(STBR)
> 
>> 물리적 메모리에서의 segment table의 위치
>> 
> ### Segment-table length register(STLR)
> 
>> 프로그램이 사용하는 segment의 수 
>> 
>>> segment number s는 STLR보다 작아야한다.

## Segmentation Architecture
> ### Protection
>> 각 세그먼트 별로 protection bit가 있다.
>> 각 엔트리는
>> 
>>> Valid bit = 0이면 illegal segment
>>> 
>>> Read/Write/Execution 권한 bit
> ### Sharing
>> shared segment
>> same segment number
>>> segment는 의미 단위이기 때문에 공유와 보안에 있어 paging보다 훨씬 효과적이다.
> ### Allocation
> 
>> segment의 크기가 일정하지 않기 때문에 hole이 많이 생긴다.
>> 
>> first fit/ best fit
>> 
>> external fragmentation이 발생한다.
