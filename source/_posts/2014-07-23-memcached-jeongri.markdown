---
layout: post
title: "memcached 정리"
date: 2014-07-23 16:34:02 +0900
comments: true
categories: "memcached"
---

memcached를 공부하면서 정리.

1. Chunk와 Slab란

memcached에서 key, value를 저장하는 크기는 이 데이터를 Chunk이라는 단위에 할당한다. (기본값 48kB) Chunk 한개에는  한쌍의 key, value를 담기때문에 60kB의 Chunk에 48kb의 K-V 데이터가 들어가면 12kB는 빈공간으로 낭비된다.

Chunk는 다시 Slab이라는 단위로 묶이는데 이 Slab의 기본 값은 1MB이다. Slab의 Chunk의 사이즈 별로 Class가 나뉘는데 이 Chuck의 사이즈는 factor 크기만큼 곱해지면서 커진다. 기본 Chunk의 사이즈가 48kB이고 factor가 1.25라면 기본 Chunk 사이즈를 가지는 Slab이 class 1이고 다음 사이즈인 48*1.25=60kB의 Chunk를 가지는 Slab이 class 2이다.

<!-- more -->

그래서 item이 메모리에 할당될 때 memcached는 item에 맞는 적절한 Chunk사이즈를 가지고 있는 Slab class를 찾고 그 Slab에  item을 할당한다. 이렇게 할당되기 때문에 item이 할당될 때 메모리가 부족하다면 전체에서 LRU를 실행하는 것이 아닌 item이 들어갈 Slab class 내에서만 LRU를 실행한다.

2. memcached 실행옵션

-m : 캐시의 최대 메모리 사이즈(MB)를 지정한다.

-M : 캐시에 지정된 메모리 이상의 데이터가 들어오면 LRU 정책으로 삭제하는 대신 에러를 리턴하도록 한다.

-c : 접속할 수 있는 최대 연결 수를 지정한다. (기본 값 : 1024)

-f : Chunk 사이즈의 증가 비율을 지정한다.

-I : Slab의 기본 사이즈를 지정한다. (기본값 : 1MB, 범위 : 1kB - 128MB)

-n : 아이템 하나가 사용할 최소 크기를 지정한다. '키 + 값 + 속성 값'의 최소 크기가 된다. (기본값 : 48)

-t : 프로토콜 파싱에 사용할 스레스 수를 지정한다. 프로토콜을 파싱할 때만 사용되고 데이터를 읽고 저장할 때에는 pthread에 의해 하나만 접근할 수 있다. (기본값 : 4)

3. memcached 스레드에 대하여



