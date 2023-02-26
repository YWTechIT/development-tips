## 📍 Hard link와 Soft link(symlink)는 무엇일까?
Unix file은 data part와 filename part로 나뉜다. data part는 inode와 연결된다. inode(index node)는 Unix 파일 시스템을 설명하는 데이터 구조인데, 각각의 inode는 객체 데이터의 속성과 디스크 블록 위치를 저장하고, 파일 시스템 객체 속성에는 metadata(times of last change, access, modification)와 permission data 등의 데이터가 들어있다. filename part는 파일 이름과 파일 이름과 관련된 inode number가 있다. 두개 이상의 파일 이름이 동일한 inode number를 참고하면 hard linked 되었다고 한다.

기본적으로 file은 hard drive를 가리킨다. (편의상 hard drive는 inode라고 칭한다.) 예를 들어, file1을 생성했는데, file2를 hard link 파일로 생성했다면 file2는 file1이 가리키고 있는 정확한 지점(hard drive)을 가리키게 된다. 이때, file1을 편집하면 자동으로 file2도 편집이 되는데, 그 이유는 그들이 같은 spot(hard drive)을 가리키고 있기 때문이다. 만약, 원본 파일과 하드링크로 생성한 파일 중 original file을 지우면 어떻게 될까? file2는 file1과 관계없이 여전히 정상적으로 사용가능한 상태인데, 그 이유는 여전히 hard drive(inode)를 가리키고 있기 때문이다. 

![](https://res.cloudinary.com/ywtechit/image/upload/v1677404170/iclooyi7uywbj4ca1k7g.png)

```
// example of Hard link
        ! filename ! inode # !
        +--------------------+
                        \
                         >--------------> ! permbits, etc ! addresses !
                        /                 +---------inode-------------+
        ! othername ! inode # !
        +---------------------+
```

반면, Soft link 혹은 Symbolic link(Aka symlink)는 hard drive에 저장된 위치를 동일하게 가리키는 Hard link와 다르게 origin file의 descriptor 혹은 name을 가리킨다. (Windows의 바로가기 아이콘을 떠올리면 쉽다.) Soft link로 만든 파일은 원본파일보다 용량이 작다. 만약, 원본 파일의 용량이 5 테라바이트라고 하더라도 Symbolic link로 새로운 파일을 생성하면 원본 파일의 용량보다 매우 작은 파일을 만들 수 있다. 이렇게 Soft link는 같은 파일이라도 용량을 작게 만들 수 있는 장점이 있지만, 단점도 있다. 바로 원본파일을 제거하면 Symbolic link로 만든 파일은 사용 할 수 없는 상태가 되는데, 그 이유는 hard drive를 가리키는게 아니라 origin file을 가리키고 있기 때문이다. 

![](https://res.cloudinary.com/ywtechit/image/upload/v1677404170/nfm1ue6dw7n9z6bjqfeg.png)

```
// example of Soft link

        ! filename ! inode # !
        +--------------------+
                        \
                         .-------> ! permbits, etc ! addresses !
                                   +---------inode-------------+
                                                      /
                                                     /
                                                    /
    .----------------------------------------------'
   ( 
    '-->  !"/path/to/some/other/file"! 
          +---------data-------------+
                  /                      }
    .~ ~ ~ ~ ~ ~ ~                       }-- (redirected at open() time)
   (                                     }
    '~~> ! filename ! inode # !
         +--------------------+
                         \
                          '------------> ! permbits, etc ! addresses !
                                         +---------inode-------------+
                                                            /
                                                           /
     .----------------------------------------------------'
    (
     '->  ! data !  ! data ! etc.
          +------+  +------+ 
```

간단하게 `Hard link` 예시를 살펴보자. 먼저, `basic.file`을 생성하고 하드링크로 만든 새 파일 `hardlink.file`을 생성했다. 그리고 `ls -lia` 커맨드를 통해 inode 번호를 포함한 파일 목록 한줄씩 나타내게 했다. 그러면 첫번째 colum에 `48298642`를 볼 수 있는데, 이를 통해 `basic.file`과 `hardlink.file`이 동일한 inode와 data를 공유한다는 점을 알 수 있다. 그리고 `chmod` 커맨드로 `basic.file`의 권한을 변경한 결과 하드링크 된 `hardlink.file`의 권한도 동일하게 변경된 것을 알 수 있다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1677405332/zrpzdldzspkhtiuzjm2p.png)

이어서 `Soft link`를 살펴보자. `ln -s` 커맨드를 이용해 소프트 링크로 `softlink.file`을 생성했다. 그리고 동일하게 `ls -lia` 커맨드로 inode를 확인한 결과 `basic.file`과 inode가 다른 점을 확인 할 수 있었다.(`basic.file`: `4829642`, `softlink.file`: `48298825`) 동일한 데이터에 접근은 가능하지만, inode가 다르고, 파일 권한도 다르다. 이제 `basic.file`을 제거하고 `softlink.file`에 접근을 시도하면 접근하지 못한다는 메시지가 뜬다.(cat: softlink.file: No such file or directory) 즉, 소프트 링크를 통해 연결된 데이터에 Access가 불가능하다는 뜻이다. 그러나 `hardlink.file`에는 접근이 가능함을 알 수 있었다.

![](https://res.cloudinary.com/ywtechit/image/upload/v1677405732/tzn6qcsa2494iulwyyup.png)

### 💡 ls -lia command의 value가 궁금하면 다음을 살펴보자.

![](https://res.cloudinary.com/ywtechit/image/upload/v1677406238/qcrfxpzi9wwomg9tjc3r.png)

#### Reference 
1. <a href='https://ko.wikipedia.org/wiki/%ED%95%98%EB%93%9C_%EB%A7%81%ED%81%AC'>Hard link - WIKI</a>
2. <a href='https://ko.wikipedia.org/wiki/%EC%8B%AC%EB%B3%BC%EB%A6%AD_%EB%A7%81%ED%81%AC'>Soft link - WIKI</a>
3. <a href='https://www.youtube.com/watch?v=4-vye3QFTFo'>Hard vs Soft Links in Linux - Youtube</a> 
4. <a href='https://www.youtube.com/watch?v=lW_V8oFxQgA'>MicroNuggets: Hard Links versus Soft Links Explained - Youtube</a> 
5. <a href='https://linuxgazette.net/105/pitcher.html'>linuxgazette - The difference between hard and soft links</a> 
6. <a href='https://docs.rockylinux.org/books/admin_guide/03-commands/'>rockylinux - commands</a>

## 📍 UTM parameter는 무엇일까?
언젠가 마케팅팀과 협업 할 때 `UTM parameter` 용어가 나왔는데, 해당 용어가 무엇인지 몰라 당황한 적이 있었다. 그래서 알아보자 `what is UTM parameter`?

`UTM(Urchin Tracking Module)`은 마케팅 담당자가 트래픽 소스 및 매체 등에서 온라인 마케팅 캠페인의 효과를 추적하는데 사용되며, 5개의 URL 매개변수(query string)로 사용된다. UTM 매개변수는 트래픽을 특정 웹사이트로 안내하는 캠페인을 식별하고 브라우저의 웹사이트 세션에 기여한다. 

여담으로 `Urchin`은 `Urchin Software Corporation`에서 개발한 웹 통계 분석 프로그램이고, 웹 서버 로그 파일 콘텐츠를 분석하고 로그 데이터를 기반으로 해당 웹사이트의 트래픽 정보를 표시했다. `Urchin Software Corp`는 2005년 4월 Google에 인수되어 지금의 `Google Analytics`가 되었다.

UTM의 매개변수는 5개가 있으며, `query` 사용 순서는 상관없다. 

| query | 목적 | 예시 |
| ----- | ----- | ----- |
| utm_source | 트래픽을 보낸 사이트를 식별하며 필수 매개변수입니다. | utm_source=google |
| utm_medium | 클릭당 비용 또는 이메일과 같이 사용된 링크 유형을 식별합니다. | utm_medium=service |
| utm_campaign | 특정 제품 프로모션 또는 전략적 캠페인을 식별합니다. | utm_campaign=air_reserve |
| utm_term | 검색어를 식별합니다. | utm_term=air_reserve_overseas |
| utm_content | 배너 광고 또는 텍스트 링크와 같이 사용자를 사이트로 유도하기 위해 구체적으로 클릭한 항목을 식별합니다. (A/B 테스트 및 콘텐츠 타겟 광고에 자주 사용됩니다.) | utm_content=로고링크 |

예시 URL은 다음과 같다. `https://google.com?utm_source=google&utm_medium=service&utm_campaign=air_reserve&utm_term=air_reserve_overseas&utm_content=로고링크`

`utm_medium`에서 `CPC`, `PPC`, `CPM`을 사용하는 경우가 있는데 `CPC(Cost Per Click)`는 노출과 상관없이 클릭 횟수 당 비용을 지불하는 방식이고, `PPC(Pay Per Click)`은 소비자가 광고주의 사이트를 방문할 경우 비용을 지불하는 방식이다. 마지막으로 `CPM(Cost Per Mile)`은 노출량을 기준으로 가격을 책정하는 방식을 의미한다. 예를 들어 100회 노출, 1000회 노출 등을 기준으로 가격 책정한다.

Reference
1. <a href='https://en.wikipedia.org/wiki/UTM_parameters'>UTM_parameters - WIKI</a>
2. <a href='https://en.wikipedia.org/wiki/Urchin_(software)'>Urchin - Wiki</a>