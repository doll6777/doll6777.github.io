---
layout: post
title: (Sorting) Queue Reconstruction by Height
tags: [algorithm, programming, leetcode]
category : algorithm
---

첫번째 요소는 사람의 신장, 두번째 요소는 자신보다 더 키가 큰사람들의 숫자이다.  
이때 이것을 자신보다 큰 사람들의 숫자기준으로 정렬하는 코드를 작성하시오.  

먼저 신장을 기준으로 정렬한다. 이떄 신장이 먼저 큰순서가 앞에 와야하고, 만약 신장이 같은 경우에는
두번째 요소인 K를 오름차순으로 하여 정렬한다.  

그 후 정렬된 리스트를 순회하며 K값을 기준으로 정렬한다.  

이때 listtoArray() 안의 값에 들어가는 파라미터는 무엇일까 조금 헷갈렸었는데 아래의 글을보고 사이즈가 0인 정수 배열을 넣었다.
http://asuraiv.blogspot.com/2015/06/java-list-toarray.html

<pre class="prettyprint">
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (p1, p2) -> (p1[0] == p2[0]? p1[1]-p2[1] : p2[0]-p1[0]));
        
        List<int[]> list = new ArrayList<int[]>();
        for(int i=0; i< people.length; i++) {
            list.add(people[i][1], people[i]);
        }
        
        return list.toArray(new int[0][]);
    }
}
</pre>