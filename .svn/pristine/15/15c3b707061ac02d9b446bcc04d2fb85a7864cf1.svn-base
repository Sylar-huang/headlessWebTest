/**
 * File Created by Lenovo at 2017/9/20.
 * Copyright 2016 CMCC Corporation Limited.
 * All rights reserved.
 *
 * This software is the confidential and proprietary information of
 * ZYHY Company. ("Confidential Information").  You shall not
 * disclose such Confidential Information and shall use it only in
 * accordance with the terms of the license.
 *
 *
 * @Desc
 * @author Lenovo
 * @date 2017/9/20
 * @version
 */
/**
 * QQ自动登录，用phantomjs模拟浏览器，自动登录到QQ兴趣部落
 * @example 执行方式：cd /phantom && ./phantomjs qq.js QQ号 QQ密码
 * phantomjs下载：http://phantomjs.org/download.html
 * 安装依赖（重要，否则会报错）：
 * sudo yum install fontconfig freetype libfreetype.so.6 libfontconfig.so.1 libstdc++.so.6
 */
var page = require('webpage').create();
var fs = require("fs");
var args = require('system').args;
page.viewportSize = { width: 1920, height: 1080 };
var pageTest = require('./pageTest')
// page.settings.userAgent = 'Mozilla/5.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/600.1.3 (KHTML, like Gecko) Version/8.0 Mobile/12A4345d Safari/600.1.4';
// page.open('http://172.23.31.88:8080/manage/app/dist/index.html#/login', function(status){
var visitUrl = 'http://10.174.235.81:8090';

args.forEach(function (val, index, array) {
    console.log(args +'|'+array)
    array.forEach(function (item) {
        if(item.indexOf('url')!=-1){
            visitUrl = item.split('=')[1];
        }
    })

});



var loginToIndex = {
    args:{
        timeout:1000,
        url:visitUrl+'/manage/app/dist/index.html',
        cap: false,
        capname:'1.png'
    },
    evaluateFunction: function (_args) {
        angular.element('#username').scope().login.loginName='18888888888'//QQ号码
        angular.element('#username').scope().login.password='cmcc0000'; //QQ密码
        document.getElementById('username').value = '18888888888'
        document.getElementById('password').value = 'cmcc0000'
        // pt.check(false);
        document.getElementsByClassName('btn-login')[0].click(); //pt.check()或.click()
    }
}

var changeUrl = {
    args:{
        timeout:2000,
        url:visitUrl+'/manage/app/dist/index.html#/index/main/warnIndex',
        cap:true,
        capname:'2.png'
    },
    evaluateFunction: function (_args) {
        document.getElementsByClassName('banner')[2].click()

    }
}
var takeShot = {
    args:{
        timeout:5000,
        url:visitUrl+'/manage/app/dist/index.html#/index/main/warnIndex',
        cap:true,
        capname:'3.png'
    },
    evaluateFunction: function (_args) {
        document.getElementsByClassName('banner')[4].click()

    }
}



pageTest.openUrl(page,loginToIndex,function () {
    console.log('---------------Page index----------------------')
    pageTest.changeUrl(page,changeUrl, function () {
        console.log('----------------Page gatewaylist---------------------')
        pageTest.changeUrl(page,takeShot,function () {
            console.log('----------------Page monitorIndex------------------')
            //phantom.exit()
        })
    })
})

//
// page.open('http://172.23.31.88:8080/manage/app/dist/index.html#/login', function(status){
//     var har;
//     if (status == 'success') {
//         var tmpTime = new Date().getTime().toString()
//         console.log('test ------' + JSON.stringify(page.resources))
//         setTimeout(function() {
//             page.evaluate(function(_args) {
//                 angular.element('#username').scope().login.loginName='18888888888'//QQ号码
//                 angular.element('#username').scope().login.password='cmcc0000'; //QQ密码
//                 document.getElementById('username').value = '18888888888'
//                 document.getElementById('password').value = 'cmcc0000'
//                 // pt.check(false);
//                 document.getElementsByClassName('btn-login')[0].click(); //pt.check()或.click()
//             }, args); //要使用传参的形式将全局变量args传入到page.evaluate()
//             // setTimeout(function() {
//             //     // document.getElementsByClassName('banner')[0].click()
//                 page.evaluate(function(_args) {
//                     // location.href='http://172.23.31.88:8080/manage/app/dist/index.html#/index/main/warnIndex'
//                     location.href='http://localhost:8080/manage/app/dist/index.html#/index/main/warnIndex'
//
//                 })
//             //     setTimeout(function () {
//             //         har = createHAR(page.address, page.title, page.startTime, page.resources);
//                     page.render('warning-'+tmpTime+'.png');
//                     phantom.exit()
//                 // },10000)
//             // }, 1000);
//         }, 1000);
//     }
// });

var dir = 'logs/'
var time = new Date().getTime().toString()
var logInfo = {
    console:[],
    error:[],
    request:[],
    receive:[],
};
page.onConsoleMessage = function (msg) {
    // if (!fs.exists(path)){
    //     fs.write(dir+'console-'+time+'.log', msg,'w');
    // }
    // fs.write(dir+'console-'+time+'.log', msg,'a/+');
    logInfo.console.push(msg)
    // console.log(msg)
}
page.onResourceError = function(resourceError){
    // if (!fs.exists(path)){
    //     fs.write(dir+'error-'+time+'.log', resourceError,'w');
    // }
    // fs.write(dir+'error-'+time+'.log', resourceError,'a/+');
    logInfo.error.push(JSON.stringify(resourceError)+'\n')
    console.log(JSON.stringify(resourceError))
}
 page.onResourceRequested = function(requestData, networkRequest){
     if(requestData.method == 'POST') {
         console.log(JSON.stringify('request interface: '+ requestData.url))
         logInfo.request.push(requestData.url)
         // if (!fs.exists(dir+'postRequest-'+time+'.log')){
         //     fs.write(dir+'postRequest-'+time+'.log', requestData.url,'w');
         // }
         // fs.write(dir+'postRequest-'+time+'.log', requestData.url,'a/+');
     }
//     console.log(JSON.stringify(networkRequest))
// }
var timeout;
page.onResourceReceived = function(response){
    clearTimeout(timeout)
    timeout = setTimeout(function () {
        var content = '-----------------received-----------------------\n'+logInfo.receive+'\n'+'---------------------error----------------------\n'+ logInfo.error
        fs.write(time+'.log', content,'w')
            phantom.exit()
    },20000)

    if(response.contentType.indexOf('application/json')!=-1) {
        //if(response.status != '200') {
        // if (!fs.exists(dir+'jsonReceived-'+time+'.log')){
        //     fs.write(dir+'jsonReceived-'+time+'.log', JSON.stringify(response),'w');
        // }
        // fs.write(dir+'jsonReceived-'+time+'.log', JSON.stringify(response),'a');
        logInfo.receive.push(JSON.stringify(response) + '\n')
            // console.log(JSON.stringify(response))
        }
    }

}
page.onUrlChanged = function(targetUrl) {
    console.log('New page: ' + targetUrl);
};


// page.onLoadFinished = function(status){
//     page.render('aaa.png');
//     console.log(status)
// }