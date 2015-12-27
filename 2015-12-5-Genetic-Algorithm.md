---
layout:     post
title:      Genetic Algorithm (JS achieve)
date:       2015-12-5 12:31:19
description: How to achieve Genetic Algorithm with javascript
categories: jekyll
thumbnail: cogs
tags:
 - demo
 - action
 - carte
 - noire
---

#####This is it
```javascript
//移除数组元素
Array.prototype.remove = function(dx) { 
    if(isNaN(dx)||dx>this.length){return false;} 
    this.splice(dx,1); 
}
//十进制转二进制
function bin(n) {
	return parseInt((n).toString(2))
}
//二进制转十进制
function ten(n) {
	return parseInt(n,2);
}

//字符串数组转整数数组
function toS(n) {
	var k = [];
	for (var i = 0; i < n.length; i++) {
		k.push(parseInt(n[i],2));
	}
	return k;
}
//数组的总和
function cos(n) {
	var co = 0;
	for(var a = 0; a < n.length; a++) {
		co += n[a]
	}
	return co;
}

//遗传算法 js实现
//求sx3和的最大值

var sx3 = ['001010','001011','001110','001101',
'000010','000011','000110','000101',
'011010','011010','011010','011010',
'001010','001011','001110','001101']

console.log(cos(toS(sx3)));

main(sx3);

function main(sx3) {
	var s = [];
	var y = toS(sx3);

	//开始的和
	var co = cos(y);

	//数组中每个元素被选中的概率
	for(var k = 0; k < y.length; k++) {
		if(s.length==0) {
			s.push(y[k]/co)
		}else {
			s.push(y[k]/co+s[s.length-1]);
		}
	}

	//随机选中元素对应的下标
	var rams = [];
	for(var i = 0; i < s.length; i++) {
		var ram = Math.random();
		for(var k = 0; k < s.length; k++) {
			if(ram<s[0]) {
				rams.push(0);
				break;
			}else if(ram>s[k] && ram<s[k+1]) {
				rams.push(k+1);
				break;
			}
		}
	}

	//择优后的数组
	var newArr = [];
	for(var i = 0; i < rams.length; i++) {
		var t = rams[i];
		newArr.push(sx3[t]);
	}


	//随机配对
	var as = [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
	var sb = [];
	for(var i = 0; i < rams.length/2; i++) {
		var n1 = r();
		g(n1)
		var n2 = r();
		g(n2);
		var bu = [];
		bu.push(n1);
		bu.push(n2);
		sb.push(bu);
	}

	function g(i) {
		for(var p = 0; p < as.length; p++) {
			if(as[p] == i) {
				as.remove(p);
			}
		}
	}

	function r() {
		var rs = 0;
		while(true) {
			var num = Math.round(Math.random()*15+0);
			var flag = false;
			for(var i = 0; i < as.length; i++) {
				if(num == as[i]) {
					rs = num;
					flag = true;
				}
			}
			if(flag) break;
		}
		return rs;
	}

	var n = [];
	for(var i = 0; i < sb.length; i++) {
		var ra = sb[i][0];
		var rb = sb[i][1];
		var su1 = newArr[ra];
		var su2 = newArr[rb];
		var t = sds(su1.length);
		//非姐妹染色单体基因交换
		var s1 = ggs(su1,t);
		var s2 = ggs(su2,t);
		su1 = s1[0]+s2[1];
		su2 = s2[0]+s1[1];
		//模拟基因突变
		su1 = op(su1,sds(su1.length));
		n.push(su1);
		n.push(su2);
	}
	console.log(cos(toS(n)));
	main(n);
}



//长度为n结果为随机数1-(n-1)
function sds(n) {
	return Math.round(Math.random()*(n-2)+0)+1;
}

function op(str) {
	var s = Math.round(Math.random()*(str.length-1)+0);
	var arr = str.split('');
	for (var i = 0; i < arr.length; i++) {
		if(i==s) {
			if(arr[i] == '1') {
				arr[i] = '0';
			}else {
				arr[i] = '1';
			}
		}
	}
	return arr.join('');
}

function io(af,ag) {
	var ps = [];
	ps.push(af[0]+ag[1]);
	ps.push(ag[0]+af[1]);
	return ps;
}

function ggs(a,s) {
	var c = [];
	if(s==(a.length-2)) {
		var te = a.substring(s,a.length+1);
		c.push(a.replace(te,''));
		c.push(te);
		return c;
	}
	if(s==(a.length-1)) {
		var te = a[a,a.length-1];
		c.push(a.replace(te,''));
		c.push(te);
		return c;
	}
	var te = a.substring(s,a.length)
	c.push(a.replace(te,''));
	c.push(te);
	return c;
}
```