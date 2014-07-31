---
author: hjinu
comments: true
date: 2014-07-14 09:48:33+00:00
layout: post
slug: naver-%ec%98%a4%ed%94%88%ec%86%8c%ec%8a%a4-%ed%94%84%eb%a1%9c%ec%a0%9d%ed%8a%b8-arcus-%ec%a0%95%eb%a6%ac
title: Naver 오픈소스 Arcus 정리
wordpress_id: 61
---

네이버에서 오픈 소스 프로젝트로 공개한 Arcus를 공부하면서 알게 된 Arcus의 특징 몇가지를 정리함.

<!-- More -->

##### - Zookeeper 사용


* 서버를 관리하기 위해서 Zookeeper를 사용. 서버의 추가나 장애 발생 시 대응에 용이.

* 서버가 켜질 때엔 Zookeeper에 IP:PORT로 검색해서 자신이 서비스되어야할 코드를 찾고 이 코드에 ephemeral node를 생성. ephemeral node는 세션이 유효한 동안만 살아있기 때문에 만약에 서버에 문제가 생겨서 세션이 종료되면 node가 사라지고 이를 바탕으로 캐시 데이터의 재분배가 가능.


##### - Eager Item Expiration


* 기존의 메모리 관리 방식에서는 LRU가 실행되면 Tail에 위치한 N개의 item만 검사한다. 이 방식에 문제점이 있는데 이는 memcached의 expired된 아이템을 삭제하는 방식과 관련되어 있음.

* memcached는 item의 expired_time이 지나면 바로 메모리에서 지우는 것이 아니라 expired된 아이템에 get 요청을 했을 때 'Item이 없다' 라고 응답하고 item을 지우기 때문.

* 그래서 Tail에 위치한 N개의 item만을 검사하면 이미 expired된 item이 있음에도 불구하고 다른 item이 evict될 수 있다. 또한 expired된 item이 메모리에 계속 올라가 있기 때문에 메모리 사용량을 확인하는 데에도 어려움.

* Eager Item Expiration은 이 문제를 해결하기 위해서 Tail에 위치한 N개의 item 이 외에도 **LRU 리스트를 점진적으로 traverse하면서 N개의 item을 검사**한다.



#####- Collection 지원

* 네이버 서비스 중에서 feed를 제공하는 서비스의 요구사항에 맞춘 데이터 구조
