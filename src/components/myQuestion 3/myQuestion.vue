<template>
  <div class='myQ'>
    <el-row class=top>
      <el-col :span="4">
        <div class="grid-content" type=flex><i class='total_a'></i>
          <div><p>{{myHeader.myTotalA}}</p>
            <p>共回答问题</p></div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content"><i class=waiting_a></i>
          <div><p>{{myHeader.myWaiting}}</p>
            <p>待审核回答</p></div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content"><i class=passing_a></i>
          <div><p>{{myHeader.passingRate}}</p>
            <p>审核通过率</p></div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content"><i class=geting_a></i>
          <div><p>{{myHeader.myGet}}</p>
            <p>获得收益</p></div>
        </div>
      </el-col>
      <el-col :span="4" :offset="4">
        <div style='cursor: pointer' class="grid-content" @click='answer()'>继续去回答<i class=el-icon-arrow-right></i></div>
      </el-col>
    </el-row>
    <el-container>
      <el-aside width="165px">
        <el-menu :default-active="$route.path" class="el-menu-vertical-demo" active-text-color='#fff' :router='true'>
          <el-menu-item index="/my">
            <template slot="title">
              <span>待审核&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{myHeader.myWaiting}}</span>
            </template>
          </el-menu-item>
          <el-menu-item index="/myPassing">
            <span slot="title">审核通过&nbsp;&nbsp;{{static.staPassing}}</span>
          </el-menu-item>
          <el-menu-item index="/myFail">
            <span slot="title">审核失败&nbsp;&nbsp;{{static.staFailing}}</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <!--<el-main>-->
      <router-view></router-view>
      <!-- </el-main>-->
    </el-container>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        myHeader: {
          myTotalA: '0',
          myWaiting: '0',
          passingRate: '0',
          myGet: '0'
        },
        static: {
          staPassing: '0',
          staFailing: '0'
        },
        activeNames: ['1'],
        buttonText: '显示全文',
        showIndex: 0,
        show_a: false,
        isTogel: false,
        scroll: ''
      }
    },
    mounted () {
      this.headerInfo()
      window.addEventListener('scroll', this.menu)
    },
    methods: {
      //获取头部信息
      headerInfo(){
        var _this = this
        this.$indexServer.auditHeader()
          .then((res) => {
            console.log(res);
            if (res.data.code == 200) {
              _this.myHeader.myTotalA = res.data.data.answerNum
              _this.myHeader.myWaiting = res.data.data.audit
              _this.myHeader.passingRate = res.data.data.answerAccuracy
              _this.myHeader.myGet = res.data.data.integralTotal
              _this.static.staPassing = res.data.data.auditSuccess
              _this.static.staFailing = res.data.data.auditFailure
            }
          })
      },
      detailTogel () {
        if (this.isTogel === true) {
          this.buttonText = '显示全文'
        } else if (this.isTogel === false) {
          this.buttonText = '收起'
        }
        this.isTogel = !this.isTogel
      },
      menu() {
        var aside=document.getElementsByClassName('el-aside')[0].getElementsByClassName('el-menu')[0]
        this.scroll = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
        aside.style.top = (243 - this.scroll) +'px'
        if (this.scroll > 185) {
          aside.style.top = '56px'
        }
      },
      answer () {
        this.$router.push({name:'question'});
      },
    },
    components: {}
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .myQ {
    margin-left: -20px;
    margin-right: -20px;
    font-family "PingFang SC-Regular"
    .top {
      color #010019
      padding 10px 15% 18px
      box-shadow 0px 2px 2px rgba(210, 210, 210, 0.5)
      -webkit-box-shadow 0px 2px 2px rgba(210, 210, 210, 0.5)
      -moz-box-shadow 0px 2px 2px rgba(210, 210, 210, 0.5)
      .el-col {
        .grid-content {
          display: -webkit-flex; /* Safari */
          display: flex;
          align-items: flex-end
          i {
            display block
            margin-right 11px
          }
          .total_a {
            width 32px
            height 27px
            background url("./write.png") no-repeat center
            background-size 100% 100%
          }
          .waiting_a {
            width 29px
            height 36px
            background url("./waiting.png") no-repeat center
            background-size 100% 100%
          }
          .passing_a {
            width 32px
            height 37px
            background url("./success.png") no-repeat center
            background-size 100% 100%
          }
          .geting_a {
            width 40px
            height 43px
            background url("./get.png") no-repeat center
            background-size 100% 100%
          }
          div {
            p {
              color #010019
              font-size 18px
              &:first-child {
                font-family "PingFang SC-Medium"
                font-size 24px
                margin-bottom 7px
              }
            }
          }
        }
        &:nth-child(4) {
          .grid-content {
            div {
              p:first-child {
                color #E7331B
              }
            }
          }
        }
        &:last-child {
          .grid-content {
            justify-content: flex-end
            line-height 49px
            font-size 18px
            i {
              margin-left 4px
              margin-right 0
              line-height 49px
            }
          }
        }
      }
    }
    .el-container {
      padding 0 15%
      .el-aside {
        padding 37px 5px 5px
        .el-menu {
          box-shadow 0px 0px 5px #D6D6D6
          position fixed
          top 243px
          left 15%
          transition top 0.3s
          .el-menu-item {
            font-size 18px !important
            color #010019
            border-bottom 1px solid #D6D6D6
            height 44px
            line-height 44px
            &:hover {
              background-color #f9553f
              color #fff
            }
            &:last-child {
              border none
            }
          }
          .is-active {
            background-color #f9553f
          }
        }
      }
      .el-main {
        padding-left 10px
        position relative
        margin 20px 0
        div {
          .a_lists {
            border-top 1px solid #D2D2D2
            padding 0 24px 0
            &:last-child {
              border-bottom 1px solid #D2D2D2
            }
            .q_title {
              color #000120
              font-size 20px
              line-height 40px
              padding 10px 100px 10px 0
              position relative
              cursor pointer
              .q_static {
                font-size 18px
                color #4a4a4a
                position absolute
                right 0px
                top 10px
              }
              i {
                font-size 20px
                position absolute
                right 0
                color #4a4a4a
                font-weight bold
              }
              .up {
                top -5px
              }
              .down {
                top 28px
              }
            }
            .textBig {
              font-size 26px
              margin-top 10px
            }
            .overHiden {
              overflow: hidden;
              text-overflow: ellipsis;
              -o-text-overflow: ellipsis;
              -webkit-text-overflow: ellipsis;
              -moz-text-overflow: ellipsis;
              white-space: nowrap;
              padding 17px 100px 17px 0
            }
            .a_descript {
              margin-bottom 12px
              padding-top 12px
              font-size 18px
              line-height 26px
              color #2E2F48
              border-top 1px solid #EBEBEB
              word-break: break-all;
              display: -webkit-box;
              -webkit-line-clamp: 3;
              -webkit-box-orient: vertical;
              overflow: hidden;
            }
            .income {
              font-size 14px
              color #000120
              display: -webkit-flex; /* Safari */
              display: flex;
              align-items: center
              .gold {
                width 27px
                height 19px
                background url("./gold.png") no-repeat center
                background-size 100% 100%
                margin 0 5px 0 12px
                display inline-block
              }
              .goldNum {
                color #e7331b
                margin-right 36px
              }
              .source {
                color #adadad
                span {
                  margin 0 15px
                }
              }
            }
            .myAnswer {
              margin 25px 24px 20px
              background #e9e9e9
              box-shadow 0 0 5px #e9e9e9
              padding 27px 27px 11px
              color #000120
              .a_top {
                font-size 20px
                margin-bottom 20px
                .lamp {
                  display inline-block
                  width 19px
                  height 27px
                  background url("./myqa.png") no-repeat center
                  background-size 100% 100%
                  margin-right 10px
                  float left
                  margin-top -6px
                }
                .a_times {
                  font-size 14px
                  float right
                  margin-top 4px
                }
              }
              .a_content {
                font-size 18px
                line-height 30px
                margin-bottom 20px
              }
              .hideSome {
                word-break: break-all;
                display: -webkit-box;
                -webkit-line-clamp: 5;
                -webkit-box-orient: vertical;
                overflow: hidden;
              }
              .el-button {
                font-size 16px
                margin 0 auto 8px
                display block
                color #B4B4B4
                height 30px
                line-height 30px
                padding 0 50px
                /*margin-top 20px*/
                /*margin-bottom -15px*/
              }
            }
          }
        }
        /*审核通过*/
        .passingCheck {
          .a_lists {
            .q_title {
              margin-top 0
              padding: 13px 100px 13px 0;
              .gainGold {
                font-size 20px
                display: -webkit-flex; /* Safari */
                display: flex;
                align-items: center
                margin-bottom 5px
                em {
                  width 41px
                  height 29px
                  background url("./gold.png") no-repeat center
                  background-size 100% 100%
                  margin 0 5px 0 12px
                  display inline-block
                }
                a {
                  font-size 26px
                  color #e7331b
                }
              }
              .up {
                top 20px
              }
              .q_static {
                color #4c9ffc
                top 57px
              }
            }
            .q_title:nth-child(2) {
              padding: 17px 100px 17px 0;
            }
            .myAnswer {
              .a_top {
                .corrected {
                  display inline-block
                  width 120px
                  height 36px
                  background #8ac8f8
                  color #FFFFFF
                  border-radius 18px
                  font-size 18px
                  line-height 36px
                  text-align center
                  margin-left 20px
                }
                .lamp {
                  margin-top 0
                }
              }
            }
          }
        }
        /*审核失败*/
        .setRoles {
          .a_lists {
            .q_title {
              .q_static {
                color #f02d17
              }
            }
            .el-button--primary {
              background rgb(249, 85, 63)
              border-color #F9553F
              color #fff
              width 150px
              height 44px
              text-align center
              line-height 44px
              font-size 20px
              line-height 44px
              padding 0
              margin-top 20px
              &:hover {
                background rgba(249, 85, 63, 0.93)
              }
            }
            .myAnswer {
              background #ffefed
            }
            .errAnswer {
              .a_top {
                color #f9553f
                .lamp {
                  width 28px
                  height 28px
                  background url("./error.png") no-repeat bottom
                  background-size 86% auto
                }
                .a_times {
                  color: #000120;
                }
              }
            }
            .keyAnswer {
              background: #e9e9e9;
              .a_top {
                .lamp {
                  width 21px
                  height 25px
                  background url("./a_right.png") no-repeat center
                  background-size 100% 100%
                }
              }
            }
          }
        }
        /*分页*/
        .block {
          position absolute
          bottom 20px
          left 0
          width 100%
        }
        .el-pagination {
          margin 0 auto
          text-align center
          color #010019
          button{
            cursor pointer
          }
          .el-pager {
            li {
              font-size 16px
              cursor pointer
              &:hover {
                color #fb432b
              }
            }
            li.active {
              color #fb432b
            }
          }
        }
      }
    }
  }
</style>
