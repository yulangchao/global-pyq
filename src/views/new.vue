<template>
    <div>
        <nv-head page-type="主题"
            :show-menu="false"
            :fix-head="true"></nv-head>
        <div class="add-container">
            <div class="line">选择分类：
                <select class="add-tab" v-model="topic.tab">
                    <option value="share">分享</option>
                    <option value="ask">问答</option>
                    <option value="job">招聘</option>
                </select>
                <a class="add-btn" @click="addTopic">发布</a>
            </div>
            <div class="line">
                <input class="add-title" v-model="topic.title"
                        type="text" :class="{'err':err=='title'}"
                        placeholder="标题，字数10字以上" max-length="100"/>
            </div>
            <textarea v-model="topic.content" rows="10" class="add-content"
                :class="{'err':err=='content'}"
                placeholder='回复支持Markdown语法,请注意标记代码'>
            </textarea>
            <div style="margin-bottom: 20px">
                <input type="file" ref:file id="addimage" @change="onFileChange" multiple>
            </div>
            <canvas style="display:none;" id="myCanvas"></canvas>
            <div v-if="images.length >0">
               <ul>
                  <li class="img-review" v-for="(image,index) in images" :key="index">

                     <img class="img-responsive" alt="" width="150" height="150" :src="image" @click='delImage(key)' />
                  </li>
               </ul>
                <button @click="removeImage">移除全部图片</button>
            </div>
        </div>
    </div>
</template>

<script>
    import FileUpload from 'vue-upload-component';
    import $ from 'webpack-zepto';
    import nvHead from '../components/header.vue';
    import {
        mapGetters
    } from 'vuex';

    export default {
        beforeMount() {
            if (!window.window.sessionStorage.getItem('user')) {
                this.$router.push({
                    name: 'login'
                });
            }
        },
        data() {
            return {
                images: [],
                smallimages: [],
                topic: {
                    tab: 'share',
                    title: '',
                    content: ''
                },
                err: ''
            };
        },
        computed: {
            ...mapGetters({
                userInfo: 'getUserInfo'
            })
        },
        methods: {
            myCanvas(imageData, size) {
                var tempImage;
                var canvas = document.getElementById('myCanvas');

                var context = canvas.getContext('2d');
                var img = new Image();
                img.onload = () => {
                    if (size > 300000) {
                        canvas.width = img.width / 3;
                        canvas.height = img.height / 3;
                    } else if (size > 200000) {
                        canvas.width = img.width / 2;
                        canvas.height = img.height / 2;
                    } else {
                        canvas.width = img.width;
                        canvas.height = img.height;
                    }
                    context.drawImage(img, 0, 0, img.width, img.height, 0, 0, canvas.width, canvas.height);
                    tempImage = canvas.toDataURL('image/jpg');
                    this.smallimages.push(tempImage);
                };
                img.src = imageData;
            },
            addPic(e) {
                this.$refs.file.click();
                $('#addPic').text('123');
                $('#addimage').click();
                return false;
            },
            onFileChange(e) {
                console.log(e);
                var files = e.target.files || e.dataTransfer.files;
                if (!files.length) return;
                this.createImage(files);
            },
            createImage(file) {
                if (typeof FileReader === 'undefined') {
                    alert('您的浏览器不支持图片上传，请升级您的浏览器');
                    return false;
                }
                var vm = this;
                console.log(file);
                var leng = file.length;
                for (var i = 0; i < leng; i++) {
                    var reader = new FileReader();
                    reader.readAsDataURL(file[i]);
                    reader.onload = function(e) {
                        vm.images.push(e.target.result);
                        vm.myCanvas(e.target.result, e.total);
                    };
                }
            },
            delImage: function(index) {
                this.images.shift(index);
                this.smallimages.shift(index);
            },
            removeImage: function(e) {
                this.images = [];
                this.smallimages = [];
            },
            uploadImage: function() {
                console.log(this.images);
                var object = {
                    images: this.images
                };
                $.ajax({
                    type: 'post',
                    url: 'upload.php',
                    data: object,
                    dataType: 'json',
                    success: function(data) {
                        if (data.status) {
                            alert(data.msg);
                            return false;
                        } else {
                            alert(data.msg);
                            return false;
                        }
                    }
                });
            },
            addTopic() {
                let title = $.trim(this.topic.title);
                let contents = $.trim(this.topic.content);

                if (!title || title.length < 10) {
                    this.err = 'title';
                    return false;
                }
                if (!contents) {
                    this.err = 'content';
                    return false;
                }
                console.log(this.smallimages);
                let postData = {
                    author: {
                        loginname: this.userInfo.loginname,
                        avatar_url: this.userInfo.avatar_url
                    },
                    author_id: this.userInfo._id,
                    content: contents,
                    tab: this.topic.tab,
                    title: title,
                    visit_count: 0,
                    top: false,
                    good: true,
                    smallimages: this.smallimages
                };
                $.ajax({
                    type: 'POST',
                    url: 'http://us.richardyych.cc:1111/api/pyq',
                    data: postData,
                    dataType: 'json',
                    success: (res) => {
                        if (res) {
                            this.$router.push({
                                name: 'list'
                            });
                        }
                    },
                    error: (res) => {
                        this.$alert('wrong');
                        return false;
                    }
                });
            }
        },
        components: {
            nvHead,
            FileUpload
        }
    };
</script>

<style lang="scss">
    .add-container {
        margin-top: 50px;
        background-color: #fff;
        .line {
            padding: 10px 15px;
            border-bottom: solid 1px #d4d4d4;
            .add-btn {
                color: #fff;
                background-color: #80bd01;
                padding: 5px 15px;
                border-radius: 5px;
            }
            .add-tab {
                height: 30px;
                display: inline-block;
                width: calc(100% - 140px);
                min-width: 50%;
                font-size: 16px;
                background: transparent;
                 :after {
                    content: 'xe60e';
                }
                ;
            }
            .add-title {
                font-size: 16px;
                border: none;
                width: 100%;
                background: transparent;
                height: 25px;
            }
            .err {
                border: solid 1px red;
            }
        }
        .add-content {
            margin: 15px 2%;
            width: 96%;
            border-color: #d4d4d4;
            color: #000;
        }
        .err {
            border: solid 1px red;
        }
        .img-review{
            display: inline-block;
            margin: 10px;
        }
    }
</style>
