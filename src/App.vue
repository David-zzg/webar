<template>
    <div>
        <div id="support"></div>
        <div id="contentHolder">
            <video id="video" playsinline="">
            </video>
            <div class="video-cover">
                <h1 @click="capture">测试</h1>
                <h1 @click="record">记录</h1>
            </div>
            <canvas style="display:none; background-color:#F00;" id="canvas" width="320" height="320">
            </canvas>
            <button id="snap" style="display:none; height:50px; width:120px;">开始扫描</button>
            <img :src="src" alt="">
        </div>
        <button @click="open">开始</button>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                src: '',
                width: window.innerWidth.toString() + "px",
                height: window.innerHeight.toString() + "px"
            }
        },
        methods: {
            capture() {
                _startPat();
            },
            record() {
                var stream = this.stream;
                console.log(stream)
                var mediaSource = new MediaSource();
                mediaSource.addEventListener('sourceopen', handleSourceOpen, false);

                function handleSourceOpen(event) {
                    console.log('MediaSource opened');
                    sourceBuffer = mediaSource.addSourceBuffer('video/webm; codecs="vp8"');
                    console.log('Source buffer: ', sourceBuffer);
                }

                function handleDataAvailable(event) {
                    if (event.data && event.data.size > 0) {
                        recordedBlobs.push(event.data);
                    }
                }

                function handleStop(event) {
                    console.log('Recorder stopped: ', event);
                }

                var recordedBlobs = [];
                var mediaRecorder;
                var options = this.getMineType();
                try {
                    mediaRecorder = new MediaRecorder(stream, options);
                } catch (e) {
                    console.error('Exception while creating MediaRecorder: ' + e);
                    alert('Exception while creating MediaRecorder: '
                        + e + '. mimeType: ' + options.mimeType);
                    return;
                }
                console.log('Created MediaRecorder', mediaRecorder, 'with options', options);
                mediaRecorder.onstop = handleStop;
                mediaRecorder.ondataavailable = handleDataAvailable;
                mediaRecorder.start(10); // collect 10ms of data
                console.log('MediaRecorder started', mediaRecorder);
                setTimeout(()=>{
                    mediaRecorder.stop();
                    var blob = new Blob(recordedBlobs, { type: 'video/webm' });
                    console.log(blob);
                },3000);
            },
            getMineType() {
                var options = {mimeType: 'video/webm;codecs=vp9'};
                if (!MediaRecorder.isTypeSupported(options.mimeType)) {
                    console.log(options.mimeType + ' is not Supported');
                    options = {mimeType: 'video/webm;codecs=vp8'};
                    if (!MediaRecorder.isTypeSupported(options.mimeType)) {
                        console.log(options.mimeType + ' is not Supported');
                        options = {mimeType: 'video/webm'};
                        if (!MediaRecorder.isTypeSupported(options.mimeType)) {
                            console.log(options.mimeType + ' is not Supported');
                            options = {mimeType: ''};
                        }
                    }
                }
                return options;
            },
            open() {
                var videoObj = {"video": true, audio: false};
                this.videoElement = document.getElementById("video");
                navigator.mediaDevices.getUserMedia(videoObj).then(i => {
                    console.log(i);
                    this.stream = i;
                    console.lo
                    this.videoElement.style.display = "block",
                        this.videoElement.srcObject = i,
                        this.videoElement.onloadedmetadata = (() => {
                            // this.videoSetting = {
                            //     width: this.videoElement.offsetWidth,
                            //     height: this.videoElement.offsetHeight
                            // },
                            //     this.canvasElement.setAttribute("width", this.videoSetting.width + "px"),
                            //         this.canvasElement.setAttribute("height", this.videoSetting.height + "px"),
                            debugger;
                            window.innerWidth < window.innerHeight ? this.videoElement.offsetHeight < window.innerHeight && this.videoElement.setAttribute("height", window.innerHeight.toString() + "px") : this.videoElement.offsetWidth < window.innerWidth && this.videoElement.setAttribute("width", window.innerWidth.toString() + "px"), e(!0)

                        });
//                    window.innerWidth < window.innerHeight ? this.videoElement.offsetHeight < window.innerHeight && this.videoElement.setAttribute("height", window.innerHeight.toString() + "px") : this.videoElement.offsetWidth < window.innerWidth && this.videoElement.setAttribute("width", window.innerWidth.toString() + "px"), e(!0)
                    setTimeout(() => {
                        this.videoElement.setAttribute("height", window.innerHeight.toString() + "px");
                    })
                    const s = this.videoElement.play();
//                            s && s.then(e => {
//                            }).catch(e => {
//                                t(e)
//                            })
                }).catch(e => {
                    console.log(e)
                });
            }
        }
        ,
        created() {
//            return;
            const that = this;
            //这段代 主要是获取摄像头的视频流并显示在Video 签中
            var canvas = null, context = null, video = null;
            window.addEventListener("DOMContentLoaded", function () {
                try {
                    canvas = document.getElementById("canvas");
                    context = canvas.getContext("2d");
                    video = document.getElementById("video");

                    var videoObj = {"video": true, audio: false},
                        flag = true,
                        MediaErr = function (error) {
                            alert(error);
                            flag = false;
                            if (error.PERMISSION_DENIED) {
                                alert('用户拒绝了浏览器请求媒体的权限', '提示');
                            } else if (error.NOT_SUPPORTED_ERROR) {
                                alert('对不起，您的浏览器不支持拍照功能，请使用其他浏览器', '提示');
                            } else if (error.MANDATORY_UNSATISFIED_ERROR) {
                                alert('指定的媒体类型未接收到媒体流', '提示');
                            } else {

                                alert('系统未能获取到摄像头，请确保摄像头已正确安装。或尝试刷新页面，重试', '提示');
                            }
                        };
//获取媒体的兼容代码，目前只支持（Firefox,Chrome,Opera）
                    if (navigator.getUserMedia) {
//qq浏览器不支持
                        if (navigator.userAgent.indexOf('MQQBrowser') > -1) {
                            alert('对不起，您的浏览器不支持拍照功能，请使用其他浏览器', '提示');
                            return false;
                        }
                        this.videoElement = video
                        navigator.mediaDevices.getUserMedia(videoObj).then(i => {
                            console.log(i);
                            that.stream = i;
                            this.videoElement.style.display = "block",
                                this.videoElement.srcObject = i;
                            const s = this.videoElement.play();
//                            s && s.then(e => {
//                            }).catch(e => {
//                                t(e)
//                            })
                        }).catch(e => {
                            console.log(e)
                        });
                        return;
                        navigator.mediaDevices.getUserMedia(videoObj)
                            .then(function (stream) {
                                video.srcObject = stream;
//                                video.src = window.webkitURL.createObjectURL(stream);
                                video.play();
//                                alert(1)
                                /* 使用这个stream stream */
                            })
                            .catch(function (err) {
                                /* 处理error */
                                console.log(err);
                            });
//                        navigator.getUserMedia(videoObj, function (stream) {
//                            console.log(arguments);
//                            const url = window.webkitURL || window.URL;
//                            console.log(window.webkitURL.createObjectURL(stream));
//                            console.log(url.createObjectURL);
//                            console.log(url.createObjectURL(stream));
//                            console.log(video);
////                            alert(1);
////                            video.src = stream;
//                            video.src = url.createObjectURL(stream);
//                            video.srcObject = stream;
//                            video.play();
//                        }, MediaErr);
                    }
                    else if (navigator.webkitGetUserMedia) {
                        alert(2)
                        navigator.webkitGetUserMedia(videoObj, function (stream) {
                            video.src = window.webkitURL.createObjectURL(stream);
                            video.play();
                        }, MediaErr);
                    }
                    else if (navigator.mozGetUserMedia) {
                        navigator.mozGetUserMedia(videoObj, function (stream) {
                            video.src = window.URL.createObjectURL(stream);
                            video.play();
                        }, MediaErr);
                    }
                    else if (navigator.msGetUserMedia) {
                        navigator.msGetUserMedia(videoObj, function (stream) {
                            $(document).scrollTop($(window).height());
                            video.src = window.URL.createObjectURL(stream);
                            video.play();
                        }, MediaErr);
                    } else {
                        alert('对不起，您的浏览器不支持拍照功能，请使用其他浏览器');
                        return false;
                    }
                    if (flag) {
//                        alert('为了获得更准确的测试结果，请尽量将二维码置于框中，然后进行拍摄、扫描。 请确保浏览器有权限使用摄像功能');
                    }
//这个是拍照按钮的事件，
                    $("#snap").click(function () {
                        startPat();
                    }).show();
                } catch (e) {
                    printHtml("浏览器不支持HTML5 CANVAS");
                }
            }, false);

            //打印内容到页面
            function printHtml(content) {
                $(window.document.body).append(content + "<br/>");
            }

            //开始拍照
            function startPat() {
                setTimeout(function () {//防止调用过快
                    if (context) {
                        context.drawImage(video, 0, 0, 320, 320);
                        CatchCode();
                    }
                }, 200);
            }

            window._startPat = startPat;
            window._a = 'ddd'

            //抓屏获取图像流，并上传到服务器
            function CatchCode() {
                if (canvas != null) {
//以下开始编 数据
                    var imgData = canvas.toDataURL();
                    that.src = imgData;
//将图像转换为base64数据
                    var base64Data = imgData;//.substr(22); //在前端截取22位之后的字符串作为图像数据
//开始异步上
                    $.post("saveimg.php", {"img": base64Data}, function (result) {
                        printHtml("解析结果：" + result.data);
                        if (result.status == "success" && result.data != "") {
                            printHtml("解析结果成功！");
                        } else {
                            startPat();//如果没有解析出来则重新抓拍解析
                        }
                    }, "json");
                }
            }
        }
    }
</script>
<style>
    #video {
        position: fixed;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        width: 20%;
        height: 20%;
        display: block;
        object-fit: cover;

    }

    .video-cover {
        position: fixed;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        width: 100%;
        height: 100%;
        z-index: 2;

    }
</style>

