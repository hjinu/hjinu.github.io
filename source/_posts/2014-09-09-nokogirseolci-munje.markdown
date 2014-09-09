---
layout: post
title: "nokogiri gem 설치 문제"
date: 2014-09-09 19:31:41 +0900
comments: true
categories: 
---

rubber gem 설치를 하는 도중 nokogiri에 디펜던시가 있어서 nokogiri gem도 설치했어야 했는데
gem이 설치되어 있는데도 불구하고 bundle install 실행 시에 계속 gem을 깔라는 오류가 떠서 
거의 이틀동안 구글에 있는 모든 해결 방법을 다 해봤는데 고칠 수 없었고...

ruby 버전을 2.0.0으로 바꿨더니 모두 해결되었다... 
앞으로 최신 버전에 대해서 보수적으로 대할 것 같다..