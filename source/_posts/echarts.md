---
title: eachart的基础用法
date: 2017-05-02 13:49:18
tags: js
---


# 前言 

这是echarts.js的基础用法


<!--more--><!--more-->


# eachart


## 去掉水平线的轴


    yAxis : [
        {
            type: 'value',
            splitLine:{
                show:false,
            }
        }
    ],


如果是要虚线：

    yAxis: {
        type: 'value',
        splitLine:{
            lineStyle:{
                type:"dashed"
            }
        }
    },



## 移动上去有图标

    tooltip : {
        trigger: 'axis',
        axisPointer : {            // 坐标轴指示器，坐标轴触发有效
            type : 'none'        // 默认为直线，可选为：'line' | 'shadow'
        }
    },

## 移动上去的图片自定义字体

    tooltip : {
        formatter: function(data) {
            return `￥ ${data[0].value}`
        }
    },


## 生成的canvas占dom的对应位置

    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
    },

单位为：10（px） 10%（百分比）


## bar柱子的宽度


    series : [
        {
            name:'直接访问',
            type:'bar',
            barWidth: '6',
        }
    ]


##  x轴不显示底线


    xAxis : [
        {
            axisLine:{
                show:false
            }
        }
    ],



## x轴的刻度线不显示


    yAxis : [
        {
            axisTick: {
                show:false,
            },
        }
    ],

## x轴字体的样式大小


    xAxis : [
        {
            axisLabel:{
                textBorderColor :'#7f8fa4',
                textBorderWidth:0
            }
        }
    ],


## 地图的底色

	series:{
        //鼠标没有选中前地图的样式
        itemStyle:{
            areaColor: '#ecf3f8',
            borderColor:'transparent',
        },
        //鼠标选中后地图的样式
        emphasis: {
            itemStyle:{
                areaColor:'#2a9ff6'
            }
        },
	}



##  line图双y轴

yAxis变成连个即可：

	   yAxis: [
	                {
	                type:'value',
	                min: 0,
	                axisTick: {
	                    show:false,
	                },
	                axisLine:{
	                    onZero: false,
	                    show:false
	                },
	                axisLabel:{
	                    textBorderColor :'#7f8fa4',
	                    textBorderWidth:0,
	                    interval:0,
	                }
	            }, {
	                    type:'value',
	                    min: 0,
	                    axisTick: {
	                        show:false,
	                    },
	                    axisLine:{
	                        onZero: false,
	                        show:false
	                    },
	                    axisLabel:{
	                        textBorderColor :'#7f8fa4',
	                        textBorderWidth:0,
	                        interval:0,
	                    }
	                }
	            ],


重点来了：数据中要有人使用其中的Y轴才行  否则没有没人使用没有刻度


    series: [
        {

            yAxisIndex:0,

        },
        {
            yAxisIndex:1,
        },
    ]


## X轴上的垂直辅助线


    axisPointer:{
        show:true,
        label:{
            formatter: function (data) {
                return data.value;
            },
            color:'#2a9ff6',
            backgroundColor:'#fff',
            shadowBlur:0,
            padding: [-5, 7, 10, 7],
        },
        lineStyle:{
           color:'#44c14a'
        }
    }


## 折线图拐点，实心圆的同时 周围有白色的圆,鼠标移入的时候效果改变


    symbol: "circle",
    itemStyle: {//折线拐点样式
        color:'#44c14a',
        borderWidth: 8,
        borderColor: '#FFFFFF',
    },
    emphasis:{
        itemStyle:{
            color:'#44c14a',
            borderWidth: 2,
            borderColor: '#FFFFFF',
        }
    },


## 移动的图标文字的位置处理


	tooltip: {
	    position: function (pos, params, dom, rect, size) {
	        //高度  鼠标居中
	        let obj = {top: pos[1]-size.contentSize[1]/2,};
	        // 左侧的时候在右侧离左边10，右侧的时候在左侧离10
	        obj.left=+(pos[0] < size.viewSize[0] / 2)?pos[0]+10:pos[0]-10-size.contentSize[0];
	        return obj;
	    },
