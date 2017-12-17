1. Q：EACCES
   
   A：sudo
2. Date.parse：注意此方法的浏览器兼容性，chrome的容错要比webkit的好，在chrome下正常的，在webkit的可能就不行。
Date.parse('19 Jan 2018 00:00:00')两种都行
但是Date.parse('2018-01-19 00:00:00')在webkit下就会返回NaN
