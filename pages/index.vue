<template>
  <div id="app" style="width: 100%;">
    <link rel="stylesheet" type="text/css" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <div id="loading" v-if="WrongOritentation===true" style="position: absolute;width: 100%;height: 100%;z-index: 999999999999999;background: white;">
      <img style="margin: 0 auto;text-align: center;display: block;" height="300px" src="../static/loading.png">
      <h3 style="text-align: center;">请横屏访问...</h3>
    </div>

<!--    <div id="loading" v-if="loadingResource===false && WrongOritentation===false" style="position: absolute;width: 100%;height: 100%;z-index: 9999999999;background: white;">-->
<!--      <img style="margin: 0 auto;text-align: center;display: block;" height="200px" src="../static/loading.png">-->
<!--      <h3 style="text-align: center;">为了保证游戏质量，正在火速加载资源中...<br>请浏览器打开，横屏游玩谢谢~~~(推荐谷歌浏览器)</h3>-->

<!--    </div>-->
    <div id="gacha_load_resource" v-if="this.loadingForGacha===true" style="z-index:9999;position: absolute;width: 100%;height: 100%;background: #2b2b2b;">
      <img :src='this.loading_img' style="width: 50%;margin: 0 auto;top: 14%;left: 0;right: 0;position:absolute;">
      <span style="position: absolute;text-shadow: 0 2px #d90059, 2px 0 #d90059, -2px 0 #d90059, 0 -1px #d90059;color: white;bottom: 8%;left: 25%;margin: auto;">NOW LOADING... {{this.waittip}}</span>
      <div id="loading" style="background: white;border-radius: 10px;width: 50%;height: 2%;right: 0;left: 0;margin: auto;position:absolute;bottom: 22px;">
        <div id="loading_process" style="transition: width 1s ease-in-out;background: #ff447d;position:absolute;width: 0%;height: 100%;border-radius: 10px;"></div>
      </div>
    </div>
    <div id="gacha_result" class="gacha_result" v-if="this.ResultLayer===true" style="position: absolute;width: 100%;height: 100%;z-index: 100000000">
      <div id="gacha_result_layer" style="flex-direction: column;position:absolute;width: 80%;height: 70%;background-color: white;margin: 0 auto;top: 0;left: 0;right: 0;bottom: 0;margin: auto;display: flex;">

        <div style="position: relative;width: 100%;height: 50%;display: flex;flex-direction: row;flex-wrap: wrap;">


          <div class="card_parent" v-for="(item,k) in this.thisTimeCardList">

            <div class="gacha_result_item">

              <!--              <div style="width: 100%;height:100%;background-size:100% 100%;background-image: url('https://oosaka.cdn.animedb.cn/BDP/card-5.png');"></div>-->
              <img :src='`https://oosaka.cdn.animedb.cn/BDP/card-${item.star}.png`' class="gacha_result_item_card_border" style="z-index:2;width: 100%;height: 100%;position: absolute;">
                            <img  :src='`https://bestdori.com/assets/jp/thumb/chara/card${Math.floor(item.id/50).toString().padStart(5,"0")}_rip/${item.resource}_${item.direct_train===true?"after_training":"normal"}.png`' class="gacha_result_item_card_img">

<!--              <img  :src='`https://bestdori.com/assets/jp/thumb/chara/card${Math.floor(item.id/50).toString().padStart(5,"0")}_rip/${item.resource}_normal.png`' class="gacha_result_item_card_img">-->
              <img :src="`https://oosaka.cdn.animedb.cn/BDP/${item.type}.svg`" class="gacha_result_item_card_type" style="z-index: 99">
              <img src="https://oosaka.cdn.animedb.cn/BDP/new.png" class="gacha_result_item_card_new" style="z-index: 99"  v-if="item.new===true">
              <img :src="`https://oosaka.cdn.animedb.cn/BDP/card/band_${getCharacterNameAndBand(item.cid)[1]}.svg`"  class="gacha_result_item_card_new" style="z-index: 99;top: 0;transform:rotate(356deg);" v-if="item.new===false">

              <img :src="`https://oosaka.cdn.animedb.cn/BDP/card/${item.direct_train===true?'star_train':'star'}.png`" :class='`gacha_result_item_card_star star_pos${index}_result`' v-for="index of item.star" :key="index">

            </div>




          </div>

        </div>

      </div>
      <div class="ResultPanel">
        <button class="button_ok" @click="return_to_gacha_pool">O K</button>
        <button class="button_ok" style="background: #f3f4f4;color: #797979;" @click="loading_gacha_resource(ten_recruit===true,selectPool)">再次招募</button>
      </div>

    </div>

    <div id="gacha_stage" class="hidden" style="position: absolute;z-index: 9999999;width: 100%;height: 100%;">
      <div class="three_star hidden">
<!--/*        <img src="https://oosaka.cdn.animedb.cn/BDP/gacha_chara_bg1.png" style="position: absolute;width: 100%;height: 240px;">*/-->
        <div style="position: absolute;width: 100%;margin-left: 43%;margin-top: 8%;height: 100%;z-index: 99999999;">
          <!--          <img id="card_information_type" class="card_information_type" style="transform:rotate(0)" src="https://oosaka.cdn.animedb.cn/BDP/card/hot.png">-->

          <!--          <div id="card_information_title" class="card_information_title" style="transform:rotate(0)">{{this.card_title}}</div>-->
          <!--          <div id="card_inforamtion_character_name" class="card_inforamtion_character_name" style="transform:rotate(0)">{{ this.card_character }}</div>-->
          <div class="three_star_title" style="text-shadow: 0 0.5vh #d90059, 0.5vh 0 #d90059, -0.5vh 0 #d90059, 0 -0.25vh #d90059;z-index: 1100000000;color: white;font-size: 5vh;">
            <img  :src="`https://oosaka.cdn.animedb.cn/BDP/${this.card_type}.svg`" style="width: 5vh;z-index: 9999999;margin-left: -6vh;margin-top: 1.3vh;position: absolute;"/>{{ this.card_title }}</div>
          <div class="three_star_title" style="text-shadow: 0 0.5vh #d90059, 0.5vh 0 #d90059, -0.5vh 0 #d90059, 0 -0.25vh #d90059;z-index: 1100000000;color: white;font-size: 9vh;">
            {{ this.card_character }}</div>


        </div>

        <div class="three_star_porto" width="300px" height="auto" :style="`background-image: url(${this.card_img});`"></div>

        <div id="three_star_pos1" style="height: 5vw;width: 6vw;margin-top: -0.5%;margin-left: 73.5%;inset: 0;" class="three_star_pos1 hidden"></div>
        <div id="three_star_pos2" style="height: 5vw;width: 6vw;margin-top: -0.5%;margin-left: calc(74.5% + 8vh + 6px);inset: 0;" class="three_star_pos2 hidden"></div>
        <img id="card_new_stage" class="hidden" style="position: absolute;margin-top: calc(19%);margin-left: calc(82.5%);width: 6vw;height: 5vw;" src="https://oosaka.cdn.animedb.cn/BDP/new.png">
      </div>
      <img id="blackMaskForStage" class="hidden" src="https://oosaka.cdn.animedb.cn/BDP/black.png" style="width: 100%;height: 100%;position: absolute;z-index: 1;">
      <div id="tickets" class="tickets_fadeIn hidden" style="width: 100%;height: 100%;position: absolute;z-index: 99999; transform: rotate(353deg);">
        <img :src="this.ticketLeft" style="position: absolute;" class="tex_ticket_left">
        <img :src="this.ticketRight" style="position: absolute;" class="tex_ticket_right">

      </div>
      <div class="customers">
        <div class="customers-main"></div>
        <div class="customers-template">
          <div class="level"></div>
          <div class="stick">
            <div class="stick-body">
              <div class="illuminant"></div>
            </div>
          </div>
        </div>
      </div>

        <p id="ticket_open_button" class="hidden ticket_open_button_css" style="height: 20px;"></p>
      <img id="gacha_background_stage" :src="this.gacha_stage_background" width="100%" height="100%" style="object-fit: inherit;height: 119%;bottom: -63px;position: absolute;">
      <img src="https://oosaka.cdn.animedb.cn/BDP/mask_gacha_test.png" class="mask_gacha_in_stage jump_in_stage hidden no_light" id="jump_in_stage">
    </div>
    <div id="hidebg" class="hidden" style="background-color:rgb(17 20 24 / 32%);position: absolute;width: 100%;height: 100%;z-index: 2147483646;"></div>



    <div class="frame pool_information" style="z-index: 2147483647;" v-if="open_frame===true && selectPoolItem!==null">
    <div class="content">
      <div class="title">
        <div class="title-icon"><i class="fa-solid fa-star"></i></div><span>卡池详情</span>
      </div>
      <div class="menu">
        <div :class="['menu-item',{'active':select_menu===1}]" @click="select_menu_func(1)">卡池说明</div>
        <div :class="['menu-item',{'active':select_menu===2}]" @click="select_menu_func(2)">抽取概率</div>
        <div :class="['menu-item',{'active':select_menu===3}]" @click="select_menu_func(3)">确定抽取概率</div>
      </div>
      <div class="main">
        <div class="hider-top"></div>
        <div class="page">
          <div class="docs" style="white-space: pre-line;">
            <div class="subtitle" v-if="select_menu===1"><i class="fa-solid fa-star"></i><b>卡池概要</b></div><span v-if="select_menu===1">{{this.selectPoolItem.meta.des}}</span><div class="subtitle" v-if="select_menu===1"><i class="fa-solid fa-star"></i><b>开催时间</b></div><span  v-if="select_menu===1">{{this.selectPoolItem.time_start}}~{{this.selectPoolItem.time_end}}</span><div class="subtitle" v-if="select_menu===2"><i class="fa-solid fa-star"></i><b>成员抽取概率</b></div><table v-if="select_menu===2">
              <tbody>
              <tr v-for="j in 5" v-if="selectPoolItem['pool_percentage'][j-1]!==undefined && selectPoolItem['pool_percentage'][j-1]!==0">
                <td style="width: 100px;"><span v-for="jk in (Math.abs(j-6))">★</span></td>
                <td>{{selectPoolItem['pool_percentage'][j-1]}}%</td>
              </tr>
              </tbody>
            </table><div class="subtitle" v-if="select_menu===3"><i class="fa-solid fa-star"></i><b>概率说明</b></div><span v-if="select_menu===3">懒得写了，抽卡算法已经判断了，就不展示了</span>





            <div class="char_list"  v-for="i in 5" v-if="(select_menu===2)&& selectPoolItem.char[(i-1)]!==null && selectPoolItem.char[(i-1)].length!==0"><div></div><div v-if="i===1" class="subtitle"><i class="fa-solid fa-star"></i><b>提供成员列表</b></div><span v-for="char in (Math.abs(6-i))">★</span><span>成员</span>
              <div class="im-table">
                <table>
                  <tbody>
                  <tr v-for="item in selectPoolItem.char[(i-1)]">
                    <td id="tips"><img :src="`https://oosaka.cdn.animedb.cn/BDP/${item.t}.svg`">{{ item.n }}</td>
                    <td id="name">{{characterList[item.c]}}</td>
                    <td id="odds"><span class="up_text" v-if="mostTimeData[i-1]!==item.p">出现率UP</span><span style="width: 52.34px;display: inline-block;">{{item.p.toFixed(3)}}%</span></td>
                  </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
        <div class="hider-bottom"></div>
      </div>
      <div class="closer-pt"></div>
      <div class="closer">
        <button @click="open_frame=false">关闭</button>
      </div>
    </div>
  </div>




    <div id="setting_star" class="hidden" style="z-index: 2147483647;position: absolute;left: 0;margin: auto;width: 50%;height: 60%;background-color: white;inset: 0;padding: 10px;border-radius: 5px;animation: tipsFadeIn 1.5s;">
      <span style="border-bottom:#ff2780 2px solid;width: 100%;position: relative;display: block;padding: 0;margin: 0;">设置星石数量</span>
      <div style="display: block; margin-top: 10px;">
        <label style="float: left;top: 7px;position: relative;">输入需要的星石数量</label>
        <input type="number" v-model="star_setting" style="float:left;width: 40%;display: block;position: relative;padding: 0px;margin: 0px;height: 35px;border: none;background: #e4e4e4;border-radius: 5px;">
      </div>
      <span style="position:relative;display: inline-block;width: 100%;top: 20px;color: #ff447d;">
            本功能仅用于评估玩家所有的星石是否能在官方卡池抽到想要的卡牌
          <br>
          功能不涉及消费，请根据自己的情况随意填写
          </span>
      <div style="position: absolute;bottom: 0;padding: 5px;width: 80%;right: 0;left: 0;margin: auto;justify-content: space-around;display: flex;">
        <button style="color: #606266;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #dcdfe6;" onclick="document.getElementById('setting_star').classList.add('hidden');document.getElementById('hidebg').classList.add('hidden');" >返回</button>
        <button style="color: white;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #ff447d;" @click="saveNowStar(0,true)">设置</button>
      </div>
    </div>
    <div id="menu" class="hidden" style="inset:0;z-index: 2147483647;position: absolute;width: 78%;height: 80%;background-color: white;inset: 0;border: 0.5px solid #e4e4e4;border-radius: 10px;margin: auto;padding: 10px;animation: tipsFadeIn 1.5s;">
      <span style="border-bottom:#ff2780 2px solid;width: 100%;position: relative;display: block;padding: 0;margin: 0;">系统菜单</span>
      <div style="width: 100%;height: 80%;">
        <ul style="display: flex;list-style: none;flex-wrap: wrap;justify-content: flex-start;width: 100%;padding: 0;">
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" @click="requestFullScreen">全屏模式</li>
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" onclick="alert('免责声明，本模拟器由粉丝制作，对标bangdream抽卡系统，仅提供抽卡的过程和结果，不会影响官方的数据结果，仅供娱乐和心理安慰，请认真阅读此声明。如有侵权请联系修改~')">免责声明</li>
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" onclick="alert('这是一款bangdream日服抽卡模拟器，基本还原了抽卡环境，希望给玩家更好的抽卡体验，你有无限的模拟星石用于在模拟器中抽卡，卡池概率与官方保持一致，可用于评估自己是否能在官方卡池抽到想要的卡片。')">使用说明</li>
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" @click="clear_data">清除数据</li>
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" @click="copyright()">版权&引用</li>
          <li style="margin-bottom:1%;margin-right:1%;width: 20%;background: #e4e4e4;border-radius: 8px;padding: 1%;border: 1px solid #909399;text-align: center;color: #606266;" @click="open_source()">开源地址</li>
        </ul>
        <span style="position:relative;display: inline-block;width: 100%;bottom: -46px;color: #ff447d;">
        本模拟器仅用于邦邦抽卡模拟，非修改官方数据，只可用于评估星石是否能换取自己希望的卡片
        <br>邦邦抽卡模拟器version: v{{app_version}}
          </span>
      </div>
      <div style="position: absolute;bottom: 0;padding: 5px;width: 80%;right: 0;left: 0;margin: auto;justify-content: space-around;display: flex;">
        <button style="color: white;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #ff447d;" onclick="document.getElementById('menu').classList.add('hidden');document.getElementById('hidebg').classList.add('hidden')">确认</button>
      </div>
    </div>

    <div id="member_filter" v-if="member_filter===true" style="inset:0;z-index: 2147483647;position: absolute;width: 78%;height: 80%;background-color: white;inset: 0;border: 0.5px solid #e4e4e4;border-radius: 10px;margin: auto;padding: 10px;animation: tipsFadeIn 1.5s;">
      <span style="border-bottom:#ff2780 2px solid;width: 100%;position: relative;display: block;padding: 0;margin: 0;">成员筛选</span>
      <div class="member_filter_layout">
        <div class="member_filter_object">属性</div>
        <div class="flex_member_filter">
          <div class="member_filter_layout_object">
            <input id="powerful" v-model="filterData.attr.powerful" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="powerful" class="label_filter_checkbox"></label>
            <img class="attribute_logo" src="https://oosaka.cdn.animedb.cn/BDP/powerful.svg"><span class="filter_object_text" style="color: #ff345a;">パワフル</span>

          </div>
          <div class="member_filter_layout_object">
            <input id="cool" v-model="filterData.attr.cool" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="cool" class="label_filter_checkbox"></label>
            <img class="attribute_logo" src="https://oosaka.cdn.animedb.cn/BDP/cool.svg"><span class="filter_object_text" style="color: #4057e3;">クール</span>

          </div>
          <div class="member_filter_layout_object">
            <input id="pure" v-model="filterData.attr.pure" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="pure" class="label_filter_checkbox"></label>
            <img class="attribute_logo" src="https://oosaka.cdn.animedb.cn/BDP/pure.svg"><span class="filter_object_text" style="color: #45c527;">ピュア</span>

          </div>
          <div class="member_filter_layout_object">
            <input id="happy" v-model="filterData.attr.happy" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="happy" class="label_filter_checkbox"></label>
            <img class="attribute_logo" src="https://oosaka.cdn.animedb.cn/BDP/happy.svg"><span class="filter_object_text" style="color: #ff6600;">ハッピー</span>
          </div>

        </div>
        <div class="member_filter_object">乐队</div>
        <div class="flex_member_filter">
          <div class="member_filter_layout_object">
            <input id="popipa" v-model="filterData.band.popipa" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="popipa" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/popipa.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="aftergrow" v-model="filterData.band.aftergrow" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="aftergrow" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/aftergrow.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="pp" v-model="filterData.band.pp" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="pp" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/pp.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="r" v-model="filterData.band.r" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="r" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/r.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="hp" v-model="filterData.band.hp" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="hp" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/hp.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="mor" v-model="filterData.band.mor" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="mor" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/mor.png">

          </div>
          <div class="member_filter_layout_object">
            <input id="rsa" v-model="filterData.band.ras" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="rsa" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/rsa.png">

          </div>

          <div class="member_filter_layout_object">
            <input id="mygo" v-model="filterData.band.mygo" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="mygo" class="label_filter_checkbox"></label>
            <img class="band_logo" src="https://oosaka.cdn.animedb.cn/BDP/band_logo/mygo.png">

          </div>
        </div>
        <div class="member_filter_object">星级</div>
        <div class="flex_member_filter">
          <div class="member_filter_layout_object">
            <input id="level_5" v-model="filterData.level.level_5" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="level_5" class="label_filter_checkbox"></label>
            <div class="flex_in_level">
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>
            </div>
          </div>
          <div class="member_filter_layout_object">
            <input id="level_4" v-model="filterData.level.level_4" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="level_4" class="label_filter_checkbox"></label>
            <div class="flex_in_level">
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>

            </div>
          </div>
          <div class="member_filter_layout_object">
            <input id="level_3" v-model="filterData.level.level_3" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="level_3" class="label_filter_checkbox"></label>
            <div class="flex_in_level" style="top: 9px;">
              <div class="star_filter"></div>
              <div class="star_filter"></div>
              <div class="star_filter"></div>

            </div>
          </div>
          <div class="member_filter_layout_object">
            <input id="level_2" v-model="filterData.level.level_2" class="hide_checkbox checkbox_for_filter" type="checkbox">
            <label for="level_2" class="label_filter_checkbox"></label>
            <div class="flex_in_level" style="top: 9px;">
              <div class="star_filter"></div>
              <div class="star_filter"></div>

            </div>
          </div>
        </div>

        <div class="member_filter_object">排序</div>
        <div class="flex_member_filter">
          <div class="member_filter_layout_object">
            <input id="get_time_order" v-model="filterData.orderby" name="orderby" class="radio_filter" value="get_time_order" type="radio">
            <label for="get_time_order" class="label_filter">入手顺</label>

          </div>
          <div class="member_filter_layout_object">
            <input id="get_star_order" v-model="filterData.orderby" name="orderby" class="radio_filter" value="get_star_order" type="radio">
            <label for="get_star_order" class="label_filter">星级顺</label>

          </div>

          <div class="member_filter_layout_object">
            <input id="publish_order" v-model="filterData.orderby" name="orderby" class="radio_filter" value="publish_order" type="radio">
            <label for="publish_order" class="label_filter">配布顺</label>

          </div>
        </div>


      </div>


      <div style="justify-content: center;position: absolute;bottom: 0;padding: 5px;width: 80%;right: 0;left: 0;margin: auto;display: flex;">
        <button style="margin-right: 11px;color: #505050;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #d8d8d8;" @click="member_filter=false;addHidden('#hidebg');">放弃</button>

        <button style="color: white;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #ff447d;" @click="filter_member();member_filter=false;addHidden('#hidebg');">筛选</button>
      </div>
    </div>
    <div id="skip" v-if="skipButton===true" style="right: 0px;padding: 10px;position: absolute;width: 76px;height: 38px;z-index: 2147483647;"><button id="skipButton" style="width: 100%;height: 100%;background-color: #f3f4f4;border: 2px solid #7c7c7b;border-radius: 7px;color: #606266;font-weight: bold;"><img src="/resource/pass.png" style="width: 20px;top: 2px;position: relative;"><span style="width: 100%;display: block;top: -3px;font-size: 12px;position: relative;">スキップ</span></button></div>


    <div id="no_enough_star" class="hidden" style="z-index: 2147483647;position: absolute;left: 0;margin: auto;width: 50%;height: 60%;background-color: white;inset: 0;padding: 10px;border-radius: 5px;animation: tipsFadeIn 1.5s;">
      <span style="border-bottom:#ff2780 2px solid;width: 100%;position: relative;display: block;padding: 0;margin: 0;">模拟星石不足</span>
      <span style="position:relative;display: inline-block;width: 100%;top: 20px;color: #ff447d;text-align: center;">
            很抱歉您设置的模拟星石不够使用，可重新设置。
      </span>
      <div style="position: relative;width: 80%;left: 0;right: 0;margin: auto;">
        <div style="padding:30px;padding-bottom: 0;">
          <img src="https://oosaka.cdn.animedb.cn/BDP/star_stone.png" style="height: 30px; position: relative; left: 10px;">
          <div style="margin-left:30px;width: 100%;position:relative;display: flex;justify-content: space-around;bottom: 30px;">
            <span>必要的模拟星石</span>
            <span >{{this.requireStar}}</span>

          </div>
        </div>
        <div style="padding:30px;padding-top: 0;">
          <img src="https://oosaka.cdn.animedb.cn/BDP/star_stone.png" style="height: 30px; position: relative; left: 10px;">
          <div style="margin-left:30px;width: 100%;position:relative;display: flex;justify-content: space-around;bottom: 30px;">
            <span>剩余的模拟星石</span>
            <span >{{this.nowStar}}</span>
          </div>
        </div>
      </div>

      <div style="position: absolute;bottom: 0;padding: 5px;width: 80%;right: 0;left: 0;margin: auto;justify-content: space-around;display: flex;">
        <button style="color: #606266;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #dcdfe6;" onclick="document.getElementById('no_enough_star').classList.add('hidden');document.getElementById('hidebg').classList.add('hidden');">返回</button>
        <button style="color: white;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #ff447d;" onclick="document.getElementById('no_enough_star').classList.add('hidden');document.getElementById('setting_star').classList.remove('hidden');">设置</button>
      </div>
    </div>

    <div id="system_tips" class="hidden" style="box-shadow: 1px 3px 9px rgb(0 0 0 / 30%);border: 1px solid #0000004d;z-index: 2147483647;position: absolute;left: 0;margin: auto;width: 50%;height: 60%;background-color: white;inset: 0;padding: 10px;border-radius: 5px;animation: tipsFadeIn 1.5s;">
      <span style="border-bottom:#ff2780 2px solid;width: 100%;position: relative;display: block;padding: 0;margin: 0;">系统提示</span>
      <span style="position:relative;display: inline-block;width: 100%;top: 20px;color: #ff447d; white-space: pre-line;overflow-y: auto;height: 60%;">{{ this.system_tips }}</span>
      <div style="position: absolute;bottom: 0;padding: 5px;width: 80%;right: 0;left: 0;margin: auto;justify-content: space-around;display: flex;">
        <button style="color: white;width: 30%;height: 37px;font-size: 19px;border-radius: 5px;border: 2px solid #c0c4cc;background-color: #ff447d;" onclick="document.getElementById('system_tips').classList.add('hidden');document.getElementById('hidebg').classList.add('hidden')">确认</button>
      </div>
    </div>

    <div class="member_room" v-if="member_room===true">
      <div class="member_top_buttom" @click="outMember()">

      </div>

      <div class="member_room_layout">
        <div class="member_room_top_button">

          <button class="order_button" @click="member_filter=true;removeHidden('#hidebg')">筛选</button>
          <button class="order_button" style="margin-right: 10px;padding: 2px 10px;" @click="filterData.order='up';filter_member()" v-if="filterData.order==='down'">倒序</button>
          <button class="order_button" style="margin-right: 10px;padding: 2px 10px;" @click="filterData.order='down';filter_member()" v-if="filterData.order==='up'">正序</button>
        </div>
        <div class="member_room_list">
          <div class="card_member_layout" @click="showGetTime(card_item)" v-for="card_item in allMyCard">
            <div class="card_member_main">
              <div :class="`member_card_border member_card_level${card_item.star}`">
                <div :class="`member_card_band member_card_band_${getCharacterNameAndBand(card_item.cid)[1]}`"></div>
                <div :class="`member_card_type member_type_${card_item.type}`"></div>
              </div>
              <div :class="`member_star_${i} member_star member_star_${card_item.direct_train===true?'training':'no_train'}`" v-for="i in card_item.star"></div>
              <img :src="`https://bestdori.com/assets/jp/thumb/chara/card${Math.floor(card_item.id / 50).toString().padStart(5, '0')}_rip/${card_item.resource}_${card_item.direct_train===true?'after_training':'normal'}.png`" class="member_card_img">
            </div>
          </div>
          <div class="blocker"></div>
          <div class="blocker"></div>
          <div class="blocker"></div>
          <div class="blocker"></div>
          <div class="blocker"></div>
          <div class="blocker"></div>
        </div>
<!--        <div class="card_info_in_member" style="width: 70vw;height: 56vh;background: white;position: absolute;z-index: 100000;top: 24px;">-->
<!--          <div class="title_on_info" style="background: rgb(227, 227, 227);width: 31vw;height: 11vh;left: 2vw;position: absolute;top: 3vh;border-radius: 7px 7px 0px 0px;">-->
<!--            <img src="https://oosaka.cdn.animedb.cn/BDP/band_logo/popipa.png" class="band_logo" style="width: 12vw;height: 12vh;">-->
<!--            <div style="top: 1vh;left: 14vw;height: 9vh;width: 15vw;position: absolute;">-->
<!--              <span>测试</span>-->
<!--              <span>松原 花音</span>-->
<!--            </div>-->
<!--          </div>-->
<!--          <img src="https://oosaka.cdn.animedb.cn/BDP/test/card_normal.png" style="width: 31vw;height: 34vh;position: absolute;top: 16vh;left: 2vw;">-->
<!--        </div>-->
      </div>

    </div>





    <div v-if="this.loading_system===true" id="loading_system" style="width: 100%; height: 100%; position: absolute; z-index: 2147483647;">
      <div style="width: 20%;position: absolute;bottom: 0;font-weight: 700;color: #ff008b;background: white;padding: 5px 10px 5px 9px;border-radius: 10px;margin: 10px;right: 0;border: 3px solid #ebeef5;">NOW LOADING...</div>
    </div>

    <div style="width: 100%;height: 100%;" v-if="loadingResource===true && start_document!==true && WrongOritentation===false && member_room===false" id="GachaPool">
      <div class="button_buttom" style="position: absolute;left: calc(18% + 36px);bottom: 5px;width: 45%;height: 30px;">
        <button style="position: relative;width: 90px;margin-right: 5px;background: #f4f5f5;border: 2px solid #e4e4e4;border-radius: 10px;height: 100%;" @click="loadInformation()">查看详情</button>
        <button style="position: relative;width: 90px;margin-right: 5px;background: #f4f5f5;border: 2px solid #e4e4e4;border-radius: 10px;height: 100%;" @click="member_room=true;filter_member()">成员室</button>
      </div>
      <div v-if="this.poolKira===true" class="keep" style="position: absolute;width: 30%;height: 45px;right: 0;background: white;bottom: 138px;border-radius: 29px;">
        <span style="bottom: 36px; left: 22px; color: #ff3b79; font-weight: bold; font-size: 22px; position: absolute; text-shadow: 0 2px #ffffff, 2px 0 #ffffff, -2px 0 #ffffff, 0 -1px #ffffff;">Kira奖励</span>
        <div style="position: absolute;width: 80%;height: 35px;background: #ffe6eb;margin:auto;top:0;bottom:0;left:19px;border-radius: 29px;color: #ff3b79;line-height: 35px;text-align: center;font-weight: 700;">
          新登场☆5成员确定
        </div>
        <span style="position: absolute;width: 88%;height: 22px;left: 8%;background: rgb(91, 91, 91);top: 47px;line-height: 22px;font-size: 13px;color: white;text-align: center;border-radius: 3px;">
          还剩 {{this.kiraTimes[this.selectPool]}}  新登场五星成员确定
        </span>
      </div>
      <div id="star_coin_panel" style="position: absolute;width: 150px;background: white;border-radius: 33px 5px 5px 33px;height: 37px;right: 72px;top: 10px;">
        <div style="position: relative;height: 50%;border-bottom: #e7e7e7 dashed 1px;">
          <img src="https://oosaka.cdn.animedb.cn/BDP/star_stone.png" style="height: 80%;position: relative;left: 10px;">
          <span style="position: absolute;font-size: 13px;top: 1px;width: 41%;left: 29px;">模拟星石</span>
          <div style="border: 1px #dcdfe6 solid;border-radius: 6px;background: whitesmoke;text-align: center;width: 67px;position: relative;font-size: 12px;right: -81px;top: -22px;" onclick="document.getElementById('hidebg').classList.remove('hidden');document.getElementById('setting_star').classList.remove('hidden')">设置数量</div>
        </div>
        <div style="position: relative;height: 50%">
          <span style="font-size: 10px; height: 100%; display: table; position: relative; width: 70%; left: 33px; ">{{this.nowStar}}</span>
        </div>

      </div>
      <div id="star_coin_panel" style="position: absolute;width: 50px;height: 37px;right: 13px;top: 10px;">

        <img src="https://oosaka.cdn.animedb.cn/BDP/button.png" width="100%" height="100%" onclick="document.getElementById('hidebg').classList.remove('hidden');document.getElementById('menu').classList.remove('hidden')">
      </div>
      <p class="text_for_time">{{this.selectPoolItem!==null && this.selectPoolItem.time_end!==null?this.selectPoolItem.time_end.slice(5)+" 结束":""}} </p>
      <img style="width: 100%;position:absolute;z-index: -20;object-fit: cover;height: 100%;" :src="`${this.poolBackground}`">

      <ul style="overflow-x:hidden;top:0;position: absolute;background-color: #ffffffc9;height: 100%;bottom: 0;left: 3%;width: 14%;margin: 0;overflow-y: scroll;padding-left: 10px;padding-right: 10px;margin-bottom: 20px;">
<!--        <div style="position:absolute;top:50%;left: 50%;transform: translate(-50%,-50%);width: 90%;text-align: center;" >-->
        <li style="width:100%;display:inline-block;list-style: none;background-color: white;padding: 0% 0px 0% 0px;margin-bottom:3%;margin-top: 3%;" :class="{'active_on_pool': item.activity_id===selectPool}" v-for="item in this.gachaPool" @click="changePool(item)"><img :src=item.meta.logo_src style="width: 100%;display: block;"></li>



<!--        </div>-->
      </ul>
      <div style="position: absolute;bottom: 0;right: 0;padding: 10px;" class="bottom_layer_pool">
        <div class="button_layer_pool" style="float:left;margin-left:10px;background-color: #ff638e;border-radius: 10px;padding: 5px;padding-right: 19px;padding-left: 19px;border: white 2px solid" @click="loading_gacha_resource(false)">
          <span style="font-size: 17px;color: white;font-weight: bold;text-shadow: 0 2px #f7467f, 2px 0 #f7467f, -2px 0 #f7467f, 0 -1px #f7467f;" >1  回招募</span>
          <div style="border-radius: 8px; background-color: white; width: 100%; padding-right: 38px; transform: translate(-19px, 0); padding-top: 2px;padding-bottom: 2px;">
            <div style="background-image: url('https://oosaka.cdn.animedb.cn/BDP/star_stone.png');background-repeat: no-repeat;display: block;z-index: 1000;background-size: 100% 100%;left: 0;width: 20px;height: 20px;"></div>
          <span style="position: absolute;width: 94%;text-align: right;top: 1px;color: rgb(255, 99, 142);font-weight: 400;">250</span></div>
        </div>
        <div class="button_layer_pool" style="float:right;margin-left:10px;background-color: #ff638e;border-radius: 10px;padding: 5px;padding-right: 19px;padding-left: 19px;border: white 2px solid" @click="loading_gacha_resource(true)">
          <span style="font-size: 17px;color: white;font-weight: bold;text-shadow: 0 2px #f7467f, 2px 0 #f7467f, -2px 0 #f7467f, 0 -1px #f7467f;">10回招募</span>
          <div style="border-radius: 8px; background-color: white; width: 100%; padding-right: 38px; transform: translate(-19px, 0); padding-top: 2px;padding-bottom: 2px;">
            <div style="background-image: url('https://oosaka.cdn.animedb.cn/BDP/star_stone.png');background-repeat: no-repeat;display: block;z-index: 1000;background-size: 100% 100%;left: 0;width: 20px;height: 20px;"></div>
            <span style="position: absolute;width: 94%;text-align: right;top: 1px;color: rgb(255, 99, 142);font-weight: 400;">2500</span></div>
        </div>
      </div>
      <div style="position: absolute;right: 0;padding: 4px;" class="hidden">
        <!--      <h1>全部卡池</h1>-->
        <!--      <img src="/resource/test/pickup.png" style="height:88%;bottom:0;right:0;left:0;margin:auto;position:absolute;z-index: 0;">-->
        <button @click="requestFullScreen">全屏</button>
        <!--      <button @click="start_test" style="z-index: 1000000">进入</button>-->
        <!--      <button @click="loading_gacha_resource(false)" style="z-index: 1000000">单次招募</button>-->
        <!--      <button @click="loading_gacha_resource" style="z-index: 1000000">十次招募</button>-->
        <button @click="clear_data" style="z-index: 1000000">清除数据</button>
        <!--      <button @click="changePool(0)" style="z-index: 1000000">切换到全部卡池</button>-->
        <!--      <button @click="changePool(1)" style="z-index: 1000000">切换到常驻池</button>-->
        <button onclick="alert('免责声明，本模拟器由粉丝制作，对标bangdream抽卡系统，仅提供抽卡的过程和结果，不会影响官方的数据结果，仅供娱乐和心理安慰，请认真阅读此声明。如有侵权请联系修改~')">免责声明</button>
        <!--      <button @click="ResultLayer=true" style="z-index: 1000000">进入结果</button>-->
      </div>
    </div>
    <div id="showCard" class="showCard" v-if="start_document===true  && card_star!==5 && WrongOritentation===false" style="position: absolute;width: 100%;height: 100%;z-index: 999999999;">
      <img id="maskForRecruit" class="hidden" style="z-index: 100000000" src="https://oosaka.cdn.animedb.cn/BDP/gacha_load.png">
      <img id="blackMask" src="https://oosaka.cdn.animedb.cn/BDP/black.png" style="width: 100%;height: 100%;">
        <img class="recruit_slog_fadein recruit_slog_fadein_animate" src="https://oosaka.cdn.animedb.cn/BDP/GachaSeriffadeinMask.png">
        <h2 class="recruit_slog hidden recruit_slog_for_not_five" id="recruit_slog" >{{ this.card_slogen }}</h2>
<!--      <img :src="`https://oosaka.cdn.animedb.cn/BDP/StageLightTStar${this.lightId}.png`">-->
    </div>
    <div id="showCardFive" class="showCard" v-if="start_document===true && card_star===5 && WrongOritentation===false" style="width: 100%;height: 100%;position:absolute;z-index: 999999999;">
      <img id="blackMaskTop" class="maskForFiveStar"  src="https://oosaka.cdn.animedb.cn/BDP/black.png" style="z-index: 100000;position: relative;">
      <img id="blackMaskButtom" class="maskForFiveStar" src="https://oosaka.cdn.animedb.cn/BDP/black.png" style="position: relative;width: 100%;height: 52%;transform: translate(0,-4%);z-index: 100000;">
<!--      <img class="recruit_slog_fadein" src="https://oosaka.cdn.animedb.cn/BDP/GachaSeriffadeinMask.png">-->
      <h2 class="recruit_slog slog_animate_for_five" id="recruit_slog" style="left:0!important;text-align: center;width:100%;position:absolute;z-index: 99999999;transform:translateY(-120%) !important;">{{ this.card_slogen }}</h2>
      <!--      <img :src="`https://oosaka.cdn.animedb.cn/BDP/StageLightTStar${this.lightId}.png`">-->
    </div>
    <div id="cardLayer" v-if="cardLayer===true && WrongOritentation===false" style="width: 100%;height: 100%;position: absolute;z-index: -9999;">
      <img :src=card_img width="100%" height="100%" style="object-fit: cover;position: absolute;" id="card_img">
      <img :src=card_img width="100%" height="100%" style="object-fit: cover;position: absolute;" id="card_img_shadow" class="hidden card_img_shadow_css">
      <div id="card_information" class="card_information hidden" style="height: 100%;width: 100%;">
        <img id="card_information_background" class="card_information_background" :src="card_information_background">
        <img id="card_information_type" class="card_information_type" :src="`https://oosaka.cdn.animedb.cn/BDP/${this.card_type}.svg`">
        <img id="card_information_new" class="card_information_new hidden" src="https://oosaka.cdn.animedb.cn/BDP/new.png">
        <div id="card_information_title" class="card_information_title">{{this.card_title}}</div>
        <div id="card_inforamtion_character_name" class="card_inforamtion_character_name">{{ this.card_character }}</div>
        <div id="star_pos1" :class="`${this.trainStar} star_pos1 stars hidden`"></div>
        <div id="star_pos2" :class="`${this.trainStar} star_pos2 stars hidden`"></div>
        <div id="star_pos3" :class="`${this.trainStar} star_pos3 stars hidden`"></div>
        <div id="star_pos4" :class="`${this.trainStar} star_pos4 stars hidden`"></div>
        <div id="star_pos5" :class="`${this.trainStar} star_pos5 five-stars hidden`"></div>
      </div>
    </div>
    <script src="https://oosaka.cdn.animedb.cn/BDP/preloadjs.min.js"></script>
  </div>
</template>
<style>
@keyframes tipsFadeIn {
  0%{
    transform: scale(0.1);
  }
  10%{
    transform: scale(1);
  }

}
.gacha_result_item_card_new{
  right: 0px;
  position: absolute;
  z-index: 99;
  height: 30%;
  width: 32%;
  transform:rotate(349deg);
  left: -3px;
  top: -5px;
}
@keyframes gacha_result_layer_animate {
  0%{
    transform: translate(20%, 0);
  }
  100%{
    transform: translate(0, 0);
  }
}
#gacha_result_layer{
  animation: gacha_result_layer_animate 0.8s;
}
ul::-webkit-scrollbar{
  background-color: transparent!important;
  width: 0;
}
ul::-webkit-scrollbar-thumb{
  background-color: transparent!important;
  width: 0;
}
.gacha_result_item_card_star{
  left: 0;
  bottom: 0px;
  width: 16%;
  position: absolute;
  z-index: 999;
  height: auto;
}
.star_pos2_result{
  bottom: 16%;
}
.star_pos3_result{
  bottom: 32%;
}
.star_pos4_result{
  bottom: 48%;
}
.star_pos5_result{
  bottom: 64%;
}
.text_for_time{
  display: block;
  position: absolute;
  bottom: 0;
  left: calc(18% + 36px);
  margin-bottom: 38px;
  font-size: 13px;
  color: white;
  background: #606266;
  border-radius: 10px;
  padding: 0px 10px;

}


.member_room_top_button{
  width: 100%;
  top: -28px;
  right: 0;
  position: absolute;
}
.order_button{
  padding: 2px 25px;
  float: right;
  border-radius: 5px;
  border: 1px solid #dedede;
  color: #505050;

}


.label_filter{
  margin-left: 30px;
}
.radio_filter{
  display: none;
}
.star_filter{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card/star.png");
  background-size: 100% 100%;
  width: 20px;
  /*flex: 0 0 33.3%;*/
  height: 20px;
}
.flex_in_level{
  width: 60px;
  flex-wrap: wrap;
  display: inline-flex;
  top: 0px;
  position: relative;

}
.band_logo{
  /*width: 96px;*/
  /*width: 61%;*/
  width: 80px;
  height: auto;
}
.filter_object_text{
  position: relative;
  font-weight: bold;
  font-size: 12px;
  display: inline-block;
  margin-left: 1px;
  bottom: 8px;
}
.label_filter_checkbox{
  margin-left: 45px;
}
.hide_checkbox{
  display: none;
}
.attribute_logo{
  width: 30px;
  display: inline;
  margin-top: 5px;
}
.radio_filter+label:before{
  display: block;
  font-size: 28px;
  border: 2px solid #dedede;
  border-radius: 100px;
  position: absolute;
  height: 20px;
  content: "";
  color: #d8d8d8;
  text-align: center;
  width: 20px;
  background-color: #fafafa;
}
.radio_filter:checked+label::before{
  display: block;
  position: absolute;
  border: 2px solid #dedede;
  border-radius: 100px;
  height: 20px;
  content: "";
  text-align: center;
  width: 20px;

  font-size: 28px;
  color: white;
  background-color: #ff3b72;
}




.checkbox_for_filter:checked+label::before{
  display: block;
  position: absolute;
  height: 40px;
  content: "\2714";
  text-align: center;
  width: 40px;
  border-radius: 10px;
  font-size: 28px;
  color: white;
  background-color: #f7366f;
}
.checkbox_for_filter+label:before{
  display: block;
  font-size: 28px;
  border-radius: 10px;
  position: absolute;
  height: 40px;
  content: "\2714";
  color: #d8d8d8;
  text-align: center;
  width: 40px;
  background-color: #a0a0a0;
}
.flex_member_filter{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  padding-top: 10px;
}
.member_filter_layout{
  height: 70%;
  overflow-y: auto;

  width: 98%;
  padding: 10px;
  position: relative;
}
.member_filter_layout_object{
  flex: 0 0 25%;
  width: 140px;
  /*flex-grow: 1;*/
  margin-bottom: 15px;
}
.member_filter_object{

  display: inline;
  color: white;
  padding: 2px 10px;
  border-radius: 30px;
  background: #505050;
}





/*这里是荧光棒代码*/
.customers {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 20%;
  z-index: 100;
}
.customers .customers-main {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  perspective: 800px;
}
.customers .customers-template {
  display: none !important;
}
.customers .level {
  position: absolute;
  left: 50%;
  height: 40px;
  display: flex;
  flex-direction: row;
  transform-style: preserve-3d;
}
.customers .level .stick {
  flex: 1;
  height: 100%;
  width: auto;
}
.customers .level .stick:nth-child(n) {
  transform: translateY(0);
}
.customers .level .stick:nth-child(5n+1) {
  transform: translateY(2px);
}
.customers .level .stick:nth-child(5n+3) {
  transform: translateY(3px);
}
.customers .level .stick:nth-child(5n+4) {
  transform: translateY(4px);
}
.customers .level .stick:nth-child(7n+4) {
  transform: translateY(-2px);
}
.customers .level .stick:nth-child(7n+5) {
  transform: translateY(-3px);
}
.customers .level .stick:nth-child(7n+6) {
  transform: translateY(-4px);
}
.customers .level.l0 .stick .stick-body {
  animation: swing .25s linear infinite;
}
.slow_swing{
  animation: swing 1.8s linear infinite!important;
}
.customers .level.l1 .stick .stick-body {
  animation: swing .25s linear infinite reverse;
}
.stick .stick-body {
  position: absolute;
  transform-origin: bottom;
  height: 40px;
  width: 8px;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  overflow: hidden;
  border-radius: 8px 8px 2px 2px;
  animation-fill-mode: both !important;
  &.white {
    background: #ccc;
    box-shadow: -2px -2px 5px #fff8,
    2px -2px 5px #fff8,
    -2px -2px 20px #ccc,
    2px -2px 20px #ccc;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #fff 0, #ccc 100%);
    }
  }
  &.yellow {
    background: #dd9;
    box-shadow: -2px -2px 5px #ffd8,
    2px -2px 5px #ffd8,
    -2px -2px 20px #dd9,
    2px -2px 20px #dd9;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #ffd 0, #dd9 100%);
    }
  }
  &.red {
    background: #caa;
    box-shadow: -2px -2px 5px #fee8,
    2px -2px 5px #fee8,
    -2px -2px 20px #caa,
    2px -2px 20px #caa;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #fee 0, #caa 100%);
    }
  }
  &.green {
    background: #aca;
    box-shadow: -2px -2px 5px #efe8,
    2px -2px 5px #efe8,
    -2px -2px 20px #aca,
    2px -2px 20px #aca;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #efe 0, #aca 100%);
    }
  }
  &.blue {
    background: #99c;
    box-shadow: -2px -2px 5px #eef8,
    2px -2px 5px #eef8,
    -2px -2px 20px #99c,
    2px -2px 20px #99c;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #eef 0, #99c 100%);
    }
  }
  &.aqua {
    background: #9cc;
    box-shadow: -2px -2px 5px #dff8,
    2px -2px 5px #dff8,
    -2px -2px 20px #9cc,
    2px -2px 20px #9cc;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #dff 0, #9cc 100%);
    }
  }
  &.plum {
    background: #cac;
    box-shadow: -2px -2px 5px #fef8,
    2px -2px 5px #fef8,
    -2px -2px 20px #cac,
    2px -2px 20px #cac;
    .illuminant {
      background: radial-gradient(ellipse at bottom, #fef 0, #cac 100%);
    }
  }
  &.star5 {
    border-radius: 8px;
    &.white {
      background: #fff;
      box-shadow: -1px -1px 5px #fd9,
      1px -1px 5px #fd9,
      1px 1px 5px #fd9,
      -1px 1px 5px #fd9,
      -4px -4px 10px #f70,
      4px -4px 10px #f70,
      4px 4px 10px #f70,
      -4px 4px 10px #f70;
    }
    &.yellow {
      background: #fe5;
      box-shadow: -1px -1px 5px #ff6,
      1px -1px 5px #ff6,
      1px 1px 5px #ff6,
      -1px 1px 5px #ff6,
      -4px -4px 10px #f90,
      4px -4px 10px #f90,
      4px 4px 10px #f90,
      -4px 4px 10px #f90;
    }
    &.red {
      background: #fb4;
      box-shadow: -1px -1px 5px #fc5,
      1px -1px 5px #fc5,
      1px 1px 5px #fc5,
      -1px 1px 5px #fc5,
      -4px -4px 10px #f70,
      4px -4px 10px #f70,
      4px 4px 10px #f70,
      -4px 4px 10px #f70;
    }
    &.green {
      background: #fff;
      box-shadow: -1px -1px 5px #eea,
      1px -1px 5px #eea,
      1px 1px 5px #eea,
      -1px 1px 5px #eea,
      -4px -4px 10px #cc4,
      4px -4px 10px #cc4,
      4px 4px 10px #cc4,
      -4px 4px 10px #cc4;
    }
    &.blue {
      background: #fff;
      box-shadow: -1px -1px 5px #daf,
      1px -1px 5px #daf,
      1px 1px 5px #daf,
      -1px 1px 5px #daf,
      -4px -4px 10px #95f,
      4px -4px 10px #95f,
      4px 4px 10px #95f,
      -4px 4px 10px #95f;
    }
    &.plum {
      background: #fff;
      box-shadow: -1px -1px 5px #fcc,
      1px -1px 5px #fcc,
      1px 1px 5px #fcc,
      -1px 1px 5px #fcc,
      -4px -4px 10px #f77,
      4px -4px 10px #f77,
      4px 4px 10px #f77,
      -4px 4px 10px #f77;
    }
    .illuminant {
      background: #fff0;
    }
  }
}
.stick .stick-body .illuminant {
  position: absolute;
  left: -50%;
  bottom: 0;
  width: 200%;
  height: 100%;
}
@keyframes swing {
  25% {
    transform: rotate(20deg);
  }
  75% {
    transform: rotate(-20deg);
  }
  50%, 100% {
    transform: rotate(0deg);
  }
}










@font-face {
  font-family: "Danske Text";
  font-weight: 400;
  font-style: normal;
  font-display: swap;
  src: url(https://bangdream-gacha.animedb.cn/fonts/dk_text-webfont.woff2) format("woff2");
}

.frame * {
  font-family: "Danske Text","Helvetica Neue",Helvetica,Arial,PingFangTC-Light,"Microsoft YaHei",微软雅黑,"STHeiti Light",STXihei,"华文细黑",Heiti,黑体,sans-serif;
}

.subtitle * {
  font-family: initial;
}
i {
  font-family: initial;
}
#tips{
  font-family: initial;
}
#name{
  font-family: initial;
}


::-webkit-scrollbar {
  width: 8px;
  background: #eee;
  border-radius: 8px;
}
::-webkit-scrollbar-thumb {
  border: 2px solid transparent;
  border-radius: 8px;
  background-color: #888;
  background-clip: padding-box;
  -webkit-background-clip: padding-box;
}
.cover {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #0008;
  z-index: 2;
}
.frame {
  position: fixed;
  top: 50%;
  left: 50%;
  width: 70%;
  height: 75%;
  max-height: 80vw !important;
  transform: translate(-50%, -50%);
  background: #fff;
  z-index: 3;
  border-radius: 0.75rem;
  padding: 1.5rem;
}
.frame .content {
  display: flex;
  position: relative;
  width: 100%;
  height: 100%;
  flex-direction: column;
}
.frame .content .title {
  position: relative;
  width: 100%;
  height: 1.75rem;
  font-size: 1.25rem;
  padding-bottom: 9px;
  &::after {
    content: "";
    position: absolute;
    right: 0;
    bottom: 0;
    width: calc(100% - 1.5rem);
    height: 3px;
    background: #ff3b73;
  }
}
.frame .content .title span {
  margin-left: 0.5rem;
  font-weight: 520;
}
.frame .content .menu {
  display: flex;
  width: calc(100% - 0.4rem);
  height: 1.6rem;
  background: #ff5584;
  border-radius: 2rem;
  margin: 16px 0 8px 0;
  padding: 0.2rem;
  gap: 0.4rem;
}
.frame .content .menu .menu-item {
  flex: 1;
  text-align: center;
  width: auto;
  height: 1.2rem;
  font-size: 0.8rem;
  padding: 0.2rem;
  color: #fff;
  border-radius: 1.5rem;
  cursor: pointer;
}
.frame .content .menu .menu-item.active {
  background: #fff;
  color: #ff5584;
}
.frame .content .main {
  position: relative;
  flex: 1;
  width: 100%;
}
.frame .content .main .hider-top {
  position: absolute;
  top: -1px;
  left: 0;
  width: calc(100% - 8px);
  height: 1rem;
  background: linear-gradient(#fff, #fff0);
  z-index: 5;
}
.frame .content .main .hider-bottom {
  position: absolute;
  left: 0;
  bottom: -1px;
  width: calc(100% - 8px);
  height: 1rem;
  background: linear-gradient(#fff0, #fff);
  z-index: 5;
}
.frame .content .main .page {
  position: absolute;
  left: 0;
  top: 0;
  width: calc(100% - 0.25rem);
  height: calc(100% - 2rem);
  overflow-y: auto;
  overflow-x: hidden;
  z-index: 4;
  padding: 1rem 0.25rem 1rem 0rem;
  font-size: 0.9rem;
  line-height: 1.2rem;
}
.frame .content .closer-pt {
  height: 2.5rem;
  margin-top: 8px;
  width: 100%;
  visibility: hidden;
}
.frame .content .closer {
  position: absolute;
  left: 50%;
  bottom: 0;
  transform: translate(-50%, 0);
  width: 9rem;
  height: 2.5rem;
  z-index: 5;
}
.frame .content .closer button {
  width: 9rem;
  height: 2.5rem;
  font-size: 1.25rem;
  border: #fff 3px solid;
  background-color: #eee;
  box-shadow: 0 0 0 1px #bbb,
  0 4px 8px -4px #444;
  border-radius: 0.25rem;
  cursor: pointer;
  &:active {
    content: "";
    background-color: #e2e2e2;
  }
}
.title-icon {
  display: inline-block;
  position: relative;
  font-size: 12px;
  width: 1.5rem;
  color: #f9bc49;
  &::before {
    content: "";
    position: absolute;
    bottom: 11px;
    left: 14px;
    width: 3px;
    height: 1rem;
    background-color: #f9bc49;
    transform: scale(1.1) rotate(27.5deg);
    border-radius: 3px;
  }
  &::after {
    content: "";
    position: absolute;
    bottom: 18px;
    left: 13px;
    width: 3px;
    height: 1rem;
    background-color: #f9bc49;
    transform: scale(1.1) rotate(27.5deg);
    border-radius: 3px;
  }
  transform: scale(80%) translate(0.25rem, 0.9rem);
}
.subtitle {
  position: relative;
  width: 100%;
  height: 0.8rem;
  margin-bottom: 1rem;
  font-size: 0.8rem;
  font-weight: 800;
  &::after {
    content: "";
    position: absolute;
    left: 0;
    bottom: -0.6rem;
    width: 100%;
    height: 100%;
    background-color: #ff3b73;
    z-index: -1;
    border-radius: 1rem;
  }
}
.subtitle i {
  color: #ff3b73;
  -webkit-text-stroke: 2px #fff;
  vertical-align: middle;
  margin-left: 0.25rem;
}
.subtitle b {
  color: #fff;
  font-weight: 1200;
  -webkit-text-stroke: 0.2px #ff3b73;
  text-shadow: #ff3b73 1px 0 0,
  #ff3b73 0 1px 0,
  #ff3b73 -1px 0 0,
  #ff3b73 0 -1px 0,
  #ff3b73 0.5px 0.5px 0,
  #ff3b73 -0.5px 0.5px 0,
  #ff3b73 -0.5px -0.5px 0,
  #ff3b73 0.5px -0.5px 0;
  vertical-align: middle;
  margin-left: 0.25rem;
}
.im-table {
  width: calc(100% - 16px);
  padding: 8px;
  border: #aaa 1px solid;
  border-radius: 0.25rem;
  margin: 8px 0 8px 0;
}
.im-table table {
  width: 100%;
}
.im-table table * {
  vertical-align: middle !important;
}
.im-table table tr {
  height: 1.5rem;
}
.im-table table td {
  height: 100%;
}
.im-table table td#tips img {
  height: 1.3rem;
  width: auto;
  vertical-align: middle;
}
.im-table table td#odds {
  text-align: right;
}
.fa-solid{
  font-family: "Font Awesome 6 Free";
}






.up_text{

  background: #ff3b73;
  color: white;
  border-radius: 10px;
  padding: 0 10px;
  margin-right: 10px;

}



.active_on_pool{
  width: 100%;
  transform: translateX(-3.5%);
  border: 5px solid #ff447d;;
}
.gacha_result_item_card_type{
  z-index: 99;
  width: 27%;
  height: 29%;
  right: 0px;
  position: absolute;
;
}
.card_parent{
  display: inline-block;width: 17%;height: 90%;margin: 1.4%;margin-bottom: 0;
}
.gacha_result_item_card_img{
  z-index:1;left:4px;width: 93%;height:97%;position: absolute;
}
.gacha_result_item_card_border{
  z-index:2;width: 100%;height: 100%;position: absolute;
}
.gacha_result_item{
  position:absolute;width: 18%;padding:0 0 18% 0;
}
.gacha_result{
  position: absolute;width: 100%;height: 100%;background-color: #fef5f2;
}
.three_star_title{
  animation-name: three_star_title_animate;
  animation-duration: 0.3s;
  animation-delay: 0.3s;
}
@keyframes three_star_title_animate {
  100%{
    text-shadow: 0 2px white, 2px 0 white, -2px 0 white, 0 -1px white;
  }

}
@keyframes three_star_proto_animate {
  0%{
    transform: translate(96%, -13%);
    opacity: 0;
    filter: blur(20px) drop-shadow(40px 0px #FF008B);
  }
  50%{
    transform: translate(-3%, -13%);
    opacity: 1;
    filter: blur(0) brightness(1) drop-shadow(40px 0px #FF008B);
  }
  80%{
    transform: translate(-3%, -13%);
    filter: blur(0) brightness(100) drop-shadow(40px 0px #FF008B);
  }
  100%{
    transform: translate(-3%, -13%);
    opacity: 1;
    filter: blur(0) brightness(1) drop-shadow(40px 0px #FF008B);
  }
}
.ResultPanel{
  position: absolute;
  bottom: 0;
  width: 80%;
  right: 0;
  left: 0;
  margin: auto;
}
.button_ok{
  float: right;
  padding: 6px;
  padding-right: 50px;
  margin-left: 10px;
  padding-left: 50px;
  background: #ff447d;
  border-radius: 9px;
  color: white;
  margin-bottom: 10px;
  border: 4px solid #fefefd;
}
.three_star_porto{
  position: absolute;
  animation-name: three_star_proto_animate;
  animation-duration: 0.7s;
  background-size: auto 100%;
  width: 100%;
  transform: translate(-3%, -13%);
  height: 100%;
  background-repeat: no-repeat;
  filter: drop-shadow(40px 0px #FF008B);
}
.three_star{
  z-index: 10000;
  position: absolute;
  height: 100%;
  background: url(https://oosaka.cdn.animedb.cn/BDP/gacha_chara_bg1.png);
  background-repeat: no-repeat;
  width: 100%;
  background-size: contain;
  transform: translate(0%, 13%);
  animation: three_star_animate 0.08s;

}
@keyframes three_star_animate {
  0%{
    transform: translate(105%,13%);
  }
  100%{
    transform: translate(0%,13%);
  }

}
.blackMaskForStageAnimate_css{
  animation: blackMaskForStageAnimate 3s;
}
@keyframes blackMaskForStageAnimate {
  0%{
    opacity: 0;
  //top: 15.7%;
  //margin-top: 30px;
  }
  10% {
    display: block;
    opacity: 1;
  }
  30%{
    display: block;
    opacity: 1;
  }
  60% {
    display: block;
    opacity: 0;
  }
  100% {
    display: block;
    opacity: 0;
  }



}
.broken_ticket_left{
  animation: broken_ticket_left_animate 1.1s;
}
.broken_ticket_right{
  animation: broken_ticket_right_animate 1.1s;
}

@keyframes broken_ticket_right_animate {
  0%{
    transform: rotate(353deg) translate(296%,71%);
    //top: 15.7%;
    //margin-top: 30px;
  }
  10% {
    transform: rotate(349deg) translate(287%,102%);
    //top: 13.7%;

    margin-left: 10px;
    //left: 66.6%;
  }
  15%{
    transform: rotate(349deg)  translate(287%,102%);

    margin-left: 10px;
    //top: 13.7%;

    //left: 66.6%;
  }
  35%{
    transform: rotate(320deg) translate(486%, -81%);
    //top: 14.7%;
  }
  100%{
    transform: rotate(320deg) translate(486%, -81%);
    //top: 14.7%;
  }




}
@keyframes broken_ticket_left_animate {
  0%{
    transform: translate(7%, 62%) rotate(353deg);


  }
  10% {
    transform: translate(7%, 62%) rotate(362deg);

  }
  15%{
    transform: translate(7%, 62%) rotate(362deg);

  }
  35%{
    transform: rotate(378deg) translate(-81%, -100%);

  }
  100%{
    transform: rotate(378deg) translate(-81%, -100%);

  }



}
.broken_ticket{
  animation: broken_ticket_animate 2s;
}
@keyframes broken_ticket_animate {
  0%{
    transform:rotateY(0) rotate(353deg) scale(1);

  }
  10%{
    transform:rotateY(0) rotate(353deg) scale(1.2);
  }
  25%{
    transform:rotateY(0) rotate(353deg) scale(0.8);
  }
  50%{
    transform:rotateY(0) rotate(353deg) scale(1);
  }
  100%{
    transform:rotateY(0) rotate(353deg) scale(1);
  }



}
@keyframes tickets_fadeIn_animate {
  0%{
    transform:rotateY(0) rotate(353deg) scale(0.2);

  }
  100%{
    transform:rotateX(1080deg) rotate(353deg) scale(1);
  }



}
.ticket_open_button_css{
  color: white;
  background: #ff2780;
  border-radius: 15px;
  position: absolute;
  z-index: 99999;
  padding: 3px 50px 4px 50px;
  width: 145px;
  text-align: center;
  display: table;
  font-weight: bold;
  height: auto;
  /* top: 42px; */
  bottom: 56px;
  left: 0;
  right: 0;
  margin: 0 auto;
  animation: ticket_open_button_animate 0.5s;
}
@keyframes ticket_open_button_animate {
  0%{
    width: 30px;
  }
  100%{
    width: 145px;
  }
}
.tickets_fadeIn{
  animation: tickets_fadeIn_animate 1.5s;
}
.tex_ticket_left{
  position: absolute;
  z-index: 10000;
  width: 60%;
  /*left: 6%;*/
  transform:rotate(353deg) translate(7%, 62%)
  /*transform: rotate(353deg);*/
  /*top: 26%;*/
}
.tex_ticket_right{
  position: absolute;
  z-index: 10000;
  width: 21.6%;
  /*left: 65.6%;*/
  transform: rotate(353deg) translate(296%,71%);
  /*top: 16.7%;*/
}
@keyframes JumpOnStage {
  0%{
    width: 96px;
    height: auto;
    transform:translateY(0)
  }
  50%{
    width: 96px;
    height: 130px;
    transform:translateY(10px)
  }
  100%{
    width: 96px;
    height: auto;
    transform:translateY(0)
  }



}

.mask_gacha_in_stage{
  z-index: 88;
  position: absolute;
  width: 96px;
  bottom: -96px;
  margin: auto;
  right: 0;
  left: 0;
  top: 0;


}




/*成员室代码*/
.member_top_buttom{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/back.png");
  background-size: 100% 100%;
  top: 0;
  position: absolute;
  width: 6vw;
  height: 6vw;
  margin: 10px;
}
.member_card_band_2{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_2.svg");
}
.member_card_band_3{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_3.svg");
}
.member_card_band_8{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_8.svg");
}
.member_card_band_4{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_4.svg");
}
.member_card_band_5{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_5.svg");
}
.member_card_band_6{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_6.svg");
}
.member_card_band_7{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_7.svg");
}
.member_card_band_1{
  background:url("https://oosaka.cdn.animedb.cn/BDP/card/band_1.svg");
}
.member_card_band{
  background-size: 100% 100%;
  position: absolute;
  width: 3vw;
  height: 3vw;
}
.member_star_5{
  bottom: 56%;
}
.member_star_4{
  bottom: 42%;
}
.member_star_3{
  bottom: 28%;
}
.member_star_2{
  bottom: 14%;
}
.member_star_1{
  bottom: 0;
}
.member_star_training{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card/star_train.png");
}
.member_star_no_train{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card/star.png");
}
.member_star{
  background-size: 100% 100%;
  width: 17%;
  left: 2px;
  z-index: 99999;
  height: 17%;

  position: absolute;
}
.member_card_img{
  width: 100%;
  height: 100%;
  position: absolute;
  z-index: 998;
}
.member_card_type{

  width: 2.65vw;
  height: 2.65vw;
  right: 0.5px;
  top: 0.7px;
  background-size: 100% 100%;
  position: absolute;
}
.member_type_powerful{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/powerful.svg");
}
.member_type_pure{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/pure.svg");

}
.member_type_happy{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/happy.svg");

}
.member_type_cool{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/cool.svg");

}
.member_card_border{
  width: 100%;
  height: 100%;
  z-index: 9999;
  position: absolute;
  background-size: 100% 100%;
}
.member_card_level1{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card-1.png");
}
.member_card_level2{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card-2.png");
}
.member_card_level3{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card-3.png");
}
.member_card_level4{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card-4.png");
}
.member_card_level5{
  background-image: url("https://oosaka.cdn.animedb.cn/BDP/card-5.png");
}
.member_room_list::-webkit-scrollbar-thumb{
  border-radius: 10px;
  background: #505050;
}
.member_room_list::-webkit-scrollbar {
  background: #d1d1d1;
  width: 5px;
  border-radius: 10px;
}
.card_member_layout{
  width: 10vw;
  padding: 5px;
  flex-grow: 1;
  height: 10vw;
  background-color: transparent;
}
.card_member_layout .card_member_main {
  width: 10vw;
  border-radius: 4px;
  position: relative;
  height: 10vw;
}
.blocker {
  width: calc(10vw + 10px);
  height: 0;
  flex-grow: 1;
}


.member_room_list{
  display: flex;
  width: 100%;
  height: 100%;
  overflow-y: auto;
  flex-direction: row;
  flex-wrap: wrap;
  align-content: flex-start;

}
.member_room_layout {
  inset: 0;
  width: 70vw;
  position: absolute;
  margin: auto;
  height: 60vh;
  background-color: white;
  border-radius: 10px;
  padding: 10px;
}
.member_room{
  width: 100%;
  height: 100%;
  top: 0;
  background-position: 50% 50%;
  left: 0;
  background-image: url('https://oosaka.cdn.animedb.cn/BDP/band.png');
  position: absolute;
  background-repeat: no-repeat;
  background-size: cover;

}
/*成员室end*/




.no_light{
  filter: brightness(0);
}
.jump_in_stage{
  animation: JumpOnStage 0.5s infinite;
}
.card_img_shadow_css{
  //filter: blur(1px);
  position: absolute;
  object-fit: cover;
  opacity: 0.8;
  transform: scale(1);
  animation: CardImgShadow 0.4s;
}

.slog_animate_for_five{
  animation: slog_title_animate 3s;
}
.card_img_animate_for_five{
  animation-name: five_card_img_animate;
  animation-duration: 7.3s;
}
.maskForFiveStar{
  transform: translate(0,0);
  width: 100%;
  height: 50%;
}
.fadeoutfivecard{
  animation: fadeOutFiveCardTop 8s;

}
.fadeoutfivecardbottom{
  animation: fadeOutFiveCardBottom 8s;
}
@keyframes CardImgShadow {
  0%{
    transform: scale(1);
    opacity: 0.8;
  }
  100%{
    opacity: 0;
    transform: scale(1.2);
  }


}
@keyframes slog_title_animate {
  0%{
    font-size: 30px;
    color: red;
  }
  5%{
    filter: blur(2px);
    color: white;
    font-size: 27px;
  }
  7%{
    filter: blur(0);
    font-size: 20px
  }
  100%{
    filter: blur(0);
    font-size: 24px;
  }


}
@keyframes five_card_img_animate {
  0%{
    filter: blur(7px);
    transform: scale(2) translate(0%, 0%);
  }
  15%{
    filter: blur(2px);
    transform: scale(2) translate(0%, 0%);
  }
  35%{
    filter: blur(2px);
    transform: scale(2) translate(-5%, -4%);
  }
  45%{
    filter: blur(2px);
    transform: scale(2) translate(0%, 0%);
  }
  45%{
    filter: blur(2px);
    transform: scale(2) translate(20%, 15%);
  }
  65%{
    filter: blur(2px);
    transform: scale(2) translate(20%, 15%);
  }
  67%{
    filter: blur(2px);
    transform: scale(2) translate(-10%, -10%);
  }
  80%{
    filter: blur(2px);
    transform: scale(2) translate(-10%, -10%);
  }
  81%{
    filter: blur(5px);
    transform: scale(1.5) translate(0%, 0%);
  }
  96%{
    filter: blur(3px);
    transform: scale(1.5) translate(0%, 0%);
  }
  97%{
    filter: blur(0);
    transform: scale(1) translate(0%, 0%);
  }


}
@keyframes fadeOutFiveCardBottom {
  0%{
    transform: translate(0%, -4%);
  }
  10%{

    transform: translate(0%, 5%);
  }
  15%{
    transform: translate(0%, 65%);
  }
  80%{
    transform: translate(0%, 65%);
  }
  100%{
    transform: translate(0%, 103%);
  }
}
@keyframes fadeOutFiveCardTop {
  0%{
    transform: translate(0%, 0%);
  }
  10%{

    transform: translate(0%, -5%);
  }
  15%{
    transform: translate(0%, -65%);
  }
  80%{
    transform: translate(0%, -65%);
  }
  100%{
    transform: translate(0%, -103%);
  }
}

@keyframes CardInformationBackGroundIn {
  0%{
    transform: rotate(354deg) translate(358px, 0);
  }
  100%{
    transform: rotate(354deg) translate(0, 0);
  }
}
.card_in_animate{
  animation: CardImgIn 3s;
}
.topForFive{
  animation: MaskTop 5s;

  transform: translate(0,0);
  width: 100%;
  height: 50%;

}

@keyframes CardImgIn {
  0%{
    transform: scale(1.1);
  }
  100%{
    transform: scale(1);
  }
}
.train_stars {
  background-image: url('https://oosaka.cdn.animedb.cn/BDP/card/star_train.png')!important;
  background-size: 100%,100%;
}
.stars{
  background: url('https://oosaka.cdn.animedb.cn/BDP/card/star.png');
  width: 30px;
  position: absolute;
  bottom: 65px;
  transform: rotate(354deg);
  background-size: 100% 100%;
  height: 30px;
  display: block;
  right: 259px;
  animation: StarIn 0.25s;
}
.three_star_pos1{
  background: url('https://oosaka.cdn.animedb.cn/BDP/card/star.png');
  position: absolute;
  background-size: 100% 100%;
  display: block;
  animation-name: StarInThreePos1;
  animation-duration:  0.25s;
}
.three_star_pos2{
  background: url('https://oosaka.cdn.animedb.cn/BDP/card/star.png');
  position: absolute;
  background-size: 100% 100%;
  display: block;
  animation-name: StarInThreePos2;
  animation-duration:  0.25s;
}
@keyframes StarInThreePos2 {
  0%{
    transform: scale(1.5)
  }
  30%{
    transform: scale(1)
  }
  60%{
    transform: scale(1.5)
  }
  100%{
    transform: scale(1)

  }
}
@keyframes StarInThreePos1 {
  0%{
    transform: scale(1.5)
  }
  30%{
    transform: scale(1)
  }
  60%{
    transform: scale(1.5)
  }
  100%{
    transform: scale(1)

  }
}
.five-stars{
  background: url('https://oosaka.cdn.animedb.cn/BDP/card/star.png');
  width: 30px;
  position: absolute;
  bottom: 65px;
  transform: rotate(354deg);
  background-size: 100% 100%;
  height: 30px;
  display: block;
  right: 259px;
  animation: StarInFive 0.5s;
  animation-timing-function: ease-in-out;
}
.star_pos1{
  bottom: 93px;
  right: 229px!important;
}
.star_pos2{
  bottom: 96px;
  right: 201px!important;
}
.star_pos3{
  bottom: 99px;
  right: 173px!important;
}
.star_pos4{
  bottom: 102px;
  right: 145px!important;
}
.star_pos5{
  bottom: 105px;
  right: 117px!important;
}
.card_information_type{
  animation: CardInformationTextIn 1.1s;
  position: absolute;
  bottom: 65px;
  width: 25px;
  transform: rotate(354deg);
  right: 259px;
}
@keyframes CardInformationTextIn {
  0%{
    filter: blur(8px);
    transform: rotate(354deg) translate(358px, 0);
  }
  100%{
    filter: blur(0);
    transform: rotate(354deg) translate(0, 0);
  }
}
.card_inforamtion_character_name{
  animation: CardInformationTextIn 1.1s;
  position: absolute;
  bottom: 28px;
  font-size: 35px;
  color: white;
  transform: rotate(354deg);
  right: 91px;
  font-weight: bold;
  text-shadow: 0 2px #d90059, 2px 0 #d90059, -2px 0 #d90059, 0 -1px #d90059;
}
.card_information_title{
  animation: CardInformationTextIn 1.1s;
  position: absolute;
  bottom: 78px;
  font-size: 20px;
  color: white;
  width: 233px;
  //width: 20%;
  transform: rotate(354deg);
  right: 24px;
  text-shadow: 0 1px #d90059, 1px 0 #d90059, -1px 0 #d90059, 0 -1px #d90059;
}
.card_information{
  height: 100%;
  width: 100%;
  display: block;
  position: absolute;
  top: 0;

}
.card_information_background{

  animation: CardInformationBackGroundIn 0.8s;
width: 300px;
transform: rotate(354deg);
bottom: 11px;
right: 6px;
position: absolute;
}
.hidden{
display: none;
}
#__nuxt,#__layout,#app{
height:100%;
}
@keyframes fadeInRecruitSlog {
0%{
  transform: translate(0%, -50%);
}
100%{

  transform: translate(100%, -50%);
}
}
@keyframes fadeOutRecruitSlog {
  0% {
    transform: translate(45%, 7%);
  }
  100% {
  //transform: translate(-125%,-50%); transform: translate(-125%, -80%);
  }
}
@keyframes NewFadeIn {
  0%{
    transform: rotate(354deg) scale(1.5)
  }
  30%{
    transform: rotate(354deg) scale(1)
  }
  60%{
    transform: rotate(354deg) scale(1.5)
  }
  100%{
    transform: rotate(354deg) scale(1)

  }
}
#card_new_stage{
  animation: NewFadeInStage 0.5s;
}
@keyframes NewFadeInStage {
  0%{
    transform: scale(1.5)
  }
  30%{
    transform: scale(1)
  }
  60%{
    transform: scale(1.5)
  }
  100%{
    transform: scale(1)

  }
}
@keyframes StarIn {
  0%{
    transform: rotate(354deg) scale(1.5)
  }
  30%{
    transform: rotate(354deg) scale(1)
  }
  60%{
    transform: rotate(354deg) scale(1.5)
  }
  100%{
    transform: rotate(354deg) scale(1)

  }
}
@keyframes StarInFive {
  0%{
    transform: rotate(460deg) scale(1.8)
  }
  100%{
    transform: rotate(354deg) scale(1)

  }
}
.bottom_recruit_slog{
//height: 284% !important;
//width: 284% !important;
left: 0;
position: absolute;
top: 0;
transform: translate(45%, 7%);
width: 400% !important;
height: auto!important;
/* right: 65%; */
  //transform: translate(-38%,-104%);
  //transform: rotate(217deg) translate(100%, 10px);
  animation: fadeOutRecruitSlog 5s;
}
.recruit_slog_fadein{
  z-index: 10000;
  position: absolute;
  display: block;
  top: 50%;
  transform: translate(100%, -50%);
  width: 100%;
  height: 50%;

}
.recruit_slog_fadein_animate{
  animation: fadeInRecruitSlog 3.2s;
}
.recruit_slog{
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  position: absolute;
  color: white;
}
html,body{
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
  border: none;
}
body{


  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.card_information_new{
  bottom: 25px;
  width: 42px;
  right: 7px;
  position: absolute;
  animation: NewFadeIn 0.5s;
  transform: rotate(354deg);
}
</style>

<script>

export default {
  name: 'IndexPage',
  head: {
    style: [
      { height: '100%' }
    ],
    meta:[
      {name:"apple-mobile-web-app-capable",content:"yes"}
    ]
  },
  data:function (){
    return {
      filterData: {
        order:"down",
        orderby:"get_time_order",
        band:{
          "popipa":true,
          "r":true,
          "ras":true,
          "aftergrow":true,
          "hp":true,
          "mor":true,
          "pp":true,
          "mygo":true
        },
        "attr":{
          "happy":true,
          "powerful":true,
          "cool":true,
          "pure":true
        },
        "level":{
          "level_2": true,
          "level_3": true,
          "level_4": true,
          "level_5": true
        }
      },
      app_version:7.5,
      characterList:["","戸山香澄","花園たえ","牛込りみ","山吹沙綾",
      "市ヶ谷有咲","美竹蘭","青葉モカ","上原ひまり","宇田川巴","羽沢つぐみ",
      "弦巻こころ","瀬田薫","北沢はぐみ","松原花音","奥沢美咲","丸山彩",
      "氷川日菜","白鷺千聖","大和麻弥","若宮イヴ","湊友希那","氷川紗夜",
      "今井リサ","宇田川あこ","白金燐子","倉田ましろ","桐ヶ谷透子",
      "広町七深","二葉つくし","八潮瑠唯","和奏レイ","朝日六花",
      "佐藤ますき","鳰原令王那","珠手ちゆ","高松燈","千早愛音","要楽奈","長崎そよ","椎名立希"],
      card_information_background:"https://oosaka.cdn.animedb.cn/BDP/card/three_star_background.png",
      card_img:"",
      version:"video",
      skipResult:false,
      fadeInMusic:null,
      poolBackground:"https://oosaka.cdn.animedb.cn/BDP/test/card_normal.png",
      card_title:"",
      portal_img:"",
      ticketLeft:"",
      mostTimeData:[],
      ten_recruit:true,
      otherButtonText:"招募一次",
      ticketRight:"",
      card_slogen:"",
      card_type:"pure",
      selectPool:1,
      starMusicQue:[],
      starImg:"",
      gacha_pool_music:null,
      maskOpenForOther:false,
      card_character:"",
      loading_system:false,
      card_resourceId:"",
      rth_music:null,
      skipButton:false,
      selectPoolItem:null,
      trainStar:"",
      waittip:"请耐心等待，进度条不一定准确",
      gachaId:0,
      cardList:[],
      open_frame:false,
      historyList:{},
      taskId:[],
      kiraTimes:[],
      member_filter:false,
      preloadList:[],
      ResultLayer:false,
      cardStage:[{id:1773,star:2, name:"上原 ひまり", slog:"测试", title:"test", img_url:"https://bestdori.com/assets/jp/characters/resourceset/res008066_rip/card_after_training.png", resource:"resd004063",type:"cool"},
        {id:1771,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"happy"},
        {star:4,direct_train:true, name:"若宮 イヴ", slog:"得到", title:"叫べ、青春の今", img_url:"https://oosaka.cdn.animedb.cn/BDP/card_nxormal.png", resource:"res011074",type:"happy"},
        {id:1770,direct_train:false,star:4, name:"若宮 イヴ", slog:null, title:"叫べ、青春の今", img_url:"https://oosaka.cdn.animedb.cn/BDP/card_nxormal.png", resource:"res004063",type:"powerful"},
        {id:1769,direct_train:false,star:5, name:"上原 ひまり", slog:"１人でがんばらせたりしない。 放っておいたりしないよ\n", title:"大丈夫じゃないなら", img_url:"https://oosaka.cdn.animedb.cn/BDP/test/img.png", resource:"res004063",type:"powerful"},
        {id:1773,direct_train:true,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"pure"}
      ,        {star:4,direct_train:true, name:"若宮 イヴ", slog:"得到", title:"叫べ、青春の今", img_url:"https://oosaka.cdn.animedb.cn/BDP/card_nxormal.png", resource:"res4004063",type:"happy"},
      ],
      thisTimeCardListTest:[
        {cid:6,id:1773,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://bestdori.com/assets/jp/characters/resourceset/res008066_rip/card_after_training.png", resource:"res004063",type:"cool"},
        {cid:7,id:1770,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"cool"},
        {cid:7,id:1773,star:4, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"cool"},
        {cid:1,id:1771,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"cool"},
        {cid:2,id:1773,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"cool"},
        {cid:33,id:1773,star:2, name:"上原 ひまり", slog:null, title:"test", img_url:"https://oosaka.cdn.animedb.cn/BDP/trim_normal.png", resource:"res004063",type:"cool"}],
      thisTimeCardList:[],
      card_star:0,
      lightId:1,
      gacha_stage_background:"https://oosaka.cdn.animedb.cn/BDP/gacha_studio_BG.png",
      musicList:[],
      background_music:null,
      slogMusic:null,
      randomStorage : {},
      changeColor : false,
      theRandom : id => {
        if (!id) return Math.random();
        if (!(id in this.randomStorage)) this.randomStorage[id] = Math.random();
        return this.randomStorage[id];
      },
      nowStar:0,
      requireStar:0,
      allMyCard:[],
      skipLock:false,
      allMyCardCopy:[],
      member_room:false,
      star_setting:0,
      pool_information_for_card:[],
      caculateStar:0,
      system_tips:"",
      loadingForGacha:true,
      WrongOritentation:false,
      cardLayer:true,
      indexId:0,
      poolKira:false,
      loading_img:"",
      start_document: false,
      select_menu:1,
      GachaBackground:"../statichttps://oosaka.cdn.animedb.cn/BDP/gacha_studio_BG.png",
      loadingResource:false,
      resourceImg:[],
      buttonForOther:"一连抽",
      successImgCount:0,
      gachaPool:[],
      needResource:["StageLightTStar1.png","StageLightULStar1.png","tex_tiket_normal_L.png"
        ,"tex_tiket_normal_R.png","GachaSeriffadeinMask.png","gacha_load.png",
        "black.png","card/four_star_background.png","card/three_star_background.png",
        "card/five_star_background.png","card/hot.png","star_stone.png",
        "card/star.png","three_star_stage.png","card/star_train.png",
        "four_star_stage.png","other_star_stage.png", "five_star_stage.png",
        "gacha_chara_bg1.png", "tex_tiket_star5_R.png","tex_tiket_star5_L.png",
        "card-1.png","card-2.png","card-3.png","card-4.png","card-5.png",
        "pure.svg","happy.svg","cool.svg","powerful.svg","new.png",
        "card/band_1.svg","card/band_2.svg","card/band_3.svg","card/band_4.svg",
        "card/band_5.svg","card/band_6.svg","card/band_7.svg","card/band_8.svg","test/card_normal.png","test/card_normal2.png","band.png","back.png"
     ]
    }
  },
  mounted() {
    if (localStorage.getItem("star")===null||localStorage.getItem("star")==="null"){
      localStorage.setItem("star",2500)
      this.nowStar = 2500
    }else{
      let temp_star = localStorage.getItem("star")
      this.nowStar = parseInt(temp_star)
    }
    this.LoadHistoryList()
    this.allMyCard = this.LoadCardListData()


    document.addEventListener("click",()=>{
      if (this.gacha_pool_music===null){
        this.gacha_pool_music = new Audio("https://oosaka.cdn.animedb.cn/BDP_MUSIC/gacha_pool.mp3")
      }
      this.gacha_pool_music.src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/gacha_pool.mp3"
      this.gacha_pool_music.play()
      this.gacha_pool_music.loop = 99999999
    },{once:true})


    this.star_setting = this.nowStar
    let musicNeed = ['fadeIn.mp3',"happy.mp3","stars.mp3","gacha_bgm.mp3","gacha_pool.mp3"]
    this.loadingForGacha = true
    this.loadingResource = true;
    this.loading_img = "https://bestdori.com/assets/jp/loading/downloading_rip/comic_"+(Math.floor(Math.random()*10)+1).toString().padStart(3,'0')+".png"
    this.cardList = this.LoadCardData();
    document.body.style.position='fixed';
    for (let i=0;i<this.needResource.length;i++){
      this.preloadList.push("https://oosaka.cdn.animedb.cn/BDP/"+this.needResource[i])


    }

    for (let i=0;i<musicNeed.length;i++){

      // this.preloadList.push("https://oosaka.cdn.animedb.cn/BDP_MUSIC/"+musicNeed[i])
      this.preloadList.push("https://oosaka.cdn.animedb.cn/BDP_MUSIC/"+musicNeed[i])


    }

    let nowProcess = 0
    let success_file = 0
    let count = this.preloadList.length
    let process = 100/count

    let que = new createjs.LoadQueue(false);
    this.waittip = "加载图像资源中，请耐心等待"
    que.on("fileload",(e)=>{
      success_file+=1
      nowProcess+=process
      document.getElementById("loading_process").style.width=nowProcess+"%"
      if (success_file===this.needResource.length){
        this.waittip = "加载音频文件中，请耐心等待"
      }

    })
    que.on("complete",()=>{
      this.waittip = "加载卡池数据中"
      // let url = "http://127.0.0.1:5000"
      let url = "https://api.bangdream.animedb.cn"
      fetch(url+"/gacha/list").then((response) => response.json()).then(
        (res)=>{
          this.gachaPool = res.data
          this.selectPool = res.data[0]['activity_id']
          this.selectPoolItem = res.data[0]
          if (res.data[0].kira===true){
            if (this.kiraTimes[res.data[0].activity_id] === undefined){
              this.kiraTimes[res.data[0].activity_id] = 50
            }
          }
          this.poolKira = res.data[0].kira
          this.poolBackground = res.data[0]['meta']["img_src"]
          this.waittip = ""
          this.loadingResource = true;
          this.loadingForGacha = false;
        })

    })
    que.loadManifest(this.preloadList)
    try{
      let loading_i = new Image();
      loading_i.src = this.loading_img
    }catch (e) {

    }

    this.rth_music = new Audio("https://oosaka.cdn.animedb.cn/BDP_MUSIC/happy.mp3")
    // this.loading_gacha_resource()
    // this.changeBackground(require("../statichttps://oosaka.cdn.animedb.cn/BDP/gacha_studio_BG.png"))

  },
  methods:{
    outMember(){
      this.allMyCardCopy = [];
      this.allMyCard = this.LoadCardListData()
      this.member_room = false;
    },
    filter_function(key,reverse){
      return this.allMyCard.sort((a,b)=>{
        if (reverse){
          return b[key] - a[key]
        }else{
          return a[key] - b[key]
        }
      })
    },
    filter_member(){
      let band_arr = ["",'popipa','aftergrow','hp','pp','r','mor','ras',"mygo"]
      if (this.allMyCardCopy.length===0){
        this.allMyCardCopy = Object.assign([],this.allMyCard)
      }else{
        this.allMyCard = Object.assign([],this.allMyCardCopy)
      }
      let reverse = null;
      if (this.filterData.order==="down"){
        reverse = true
      }else{
        reverse = false
      }
      switch (this.filterData.orderby){
        case "get_time_order":
          this.allMyCard = this.filter_function("get_time",reverse)
          break

        case "get_star_order":
          this.allMyCard = this.filter_function("star",reverse)
          break
        case "publish_order":
          this.allMyCard = this.filter_function("id",reverse)
          break
      }

      //处理属性
      for(let i=this.allMyCard.length-1;i>=0;i--){

        if (this.filterData.attr[this.allMyCard[i]['type']]===false){
          this.allMyCard.splice(i,1)
          continue
        }
        if (this.filterData.band[band_arr[this.getCharacterNameAndBand(this.allMyCard[i]['cid'])[1]]]===false){
          this.allMyCard.splice(i,1)
          continue
        }
        if (this.filterData.level['level_'+this.allMyCard[i]['star']]===false){
          this.allMyCard.splice(i,1)
          continue
        }

      }





    },
    //荧光棒代码
    createCustomers(rank,slow=false) {
      this.removeCustomers();
      const main = document.querySelector(".customers .customers-main");
      const template = document.querySelector(".customers .customers-template");
      const ln = Number(Math.ceil(main.offsetHeight / 30)) + 1;
      for (let i = 0; i < ln; i++) {
        const level = template.querySelector(".level").cloneNode(true);
        let name = "l" + String(i % 2);
        level.classList.add(name);
        let gap = -Number((400 / ln).toFixed(3)) * (i - 1.75);
        level.style = `transform: translate3d(-50%, 0, ${gap}px); width: ${100 + 10 * i}%; bottom: ${main.offsetHeight / ln * i}px; z-index: ${100 + (ln - i)};`;
        const sn = Number(Math.ceil(main.offsetWidth / 80)) + Number(Math.floor(this.theRandom(`i${i}sn`) * -2)) + 3 + (rank == 5 ? 5 : 0);
        for (let j = 0; j < sn; j++) {
          const stick = template.querySelector(".stick").cloneNode(true);
          let color = (rank => {
            switch (rank) {
              case 2:
                return ["white"];
              case 3:
                return ["yellow"];
              case 4:
                let star4Color = ["white", "yellow", "red", "green", "blue", "aqua", "plum"];
                return [star4Color[Math.floor(this.theRandom((this.changeColor ? `i${i}j${j}s4c` : null)) * star4Color.length)]];
              case 5:
                let star5Color = ["white", "yellow", "red", "green", "blue", "plum"];
                return [star5Color[Math.floor(this.theRandom((this.changeColor ? `i${i}j${j}s5c` : null)) * star5Color.length)], "star5"];
              default:
                throw "Error!";
            }
          })(rank);
          stick.querySelector(".stick-body").classList.add(...color);
          if (slow===true){
            stick.querySelector(".stick-body").classList.add("slow_swing");

          }
          stick.querySelector(".stick-body").style.left = String(Number(Math.floor(this.theRandom(`i${i}j${j}lp`) * -21)) + 60) + "px";
          level.appendChild(stick);
        }
        main.appendChild(level);
      }
    },
    removeCustomers() {
  document.querySelector(".customers .customers-main").innerHTML = null;
},
    pauseCustomers() {
      let sticks = Array.from(document.querySelectorAll(".stick .stick-body"));
      sticks.forEach(element => element.style.animation = "none");
    },
    continueCustomers() {
      let sticks = Array.from(document.querySelectorAll(".stick .stick-body"));
      sticks.forEach(element => element.style.animation = element.parentNode.parentNode.classList.contains("l1") ? `swing .25s linear infinite reverse` : `swing .25s linear infinite`);
    },
    addHidden(node){
      if (document.querySelector(node)!==undefined){
        document.querySelector(node).classList.add("hidden")
      }

    },
    removeHidden(node){
      if (document.querySelector(node)!==undefined){
        document.querySelector(node).classList.remove("hidden")
      }
    },
    saveNowStar(star,ui=false){

      if (ui===true){
        star = this.star_setting
      }
      localStorage.setItem("star",star)
      this.nowStar = star
      if (ui===true){
        this.system_tips = "设置成功，请继续抽卡"
        this.addHidden("#setting_star")
        this.removeHidden("#system_tips")
      }
    },
    copyright(){
      this.system_tips = "版权申明及引用\n本模拟器仅供学习用途及娱乐，非商业用途。\n游戏资源归バンドリ！ ガールズバンド游戏所有者所有(见下文)\n\n开源后申明:本模拟器在开源后须遵循开源协议进行使用,严禁售卖或封装成任意程序插件出售(如discuz等)。\n同时严禁商用，违者将被取消本项目代码的任何使用权\n\n\n部分API和游戏图资源来源：Bestdori（感谢）\n\n感谢 梓漪Aurora、Burrito对本项目提供的支持\n\n卡牌等资源来源及版权:\n游戏名: バンドリ！ ガールズバンドパーティ！\n游戏资源等版权所有者：©BanG Dream! Project ©Craft Egg Inc. ©bushiroad All Rights Reserved.\n\n当您使用本程序的任意代码或服务时默认您已同意本协议。\n\n图片等资源来自网络，如侵权请联系修改~ "
      this.removeHidden("#system_tips")
    },
    open_source(){
      this.system_tips = "https://github.com/banGdream-gacha/frontier_vue (即将开源)"
      this.removeHidden("#system_tips")

    },
    ifCardNew(id){
      if (this.cardList.indexOf(id)===-1){
        return true;
      }else{
        return false;
      }

    },
    showGetTime(cs){
      this.system_tips = `你获得了这张卡片(角色名：${this.getCharacterNameAndBand(cs['cid'])[0]})\n来自${cs['get_from_pool_name']}\n用了${cs['get_use_time']}抽(约${(cs['get_use_time']/10).toFixed(1)}个十连)\n本次出货你耗费了${cs['get_use_time']*250}个星石，可喜可贺`
      if (cs['get_use_time'] <= 10 && cs['star']===5){
        this.system_tips += "\n\n太欧了"
      }
      this.system_tips += "\n\nPS:由于模拟器的概率和官方基本一致，如果你用了较多次数才出货不建议去硬磕这张卡。"
      this.removeHidden("#hidebg")
      this.removeHidden("#system_tips")
      // alert("你获得了这张卡片\n来自"+cardList[i]['get_from_pool_name']"池",)
      // cardList[i]['get_time']
      // cardList[i]['get_from_pool_name']
      // cardList[i]['get_use_time'] = this.getHistory(this.selectPool)
    },
    getCharacterNameAndBand(cid){
      let characterArray = []
      characterArray.push(this.characterList[cid])
      characterArray.push(Math.ceil(cid/5))
      return characterArray
    },
    AddCardData(idList){
      let temp = this.LoadCardData("Card")
      for (let i=0;i<idList.length;i++){
        if (temp.indexOf(idList[i])===-1){
          temp.push(idList[i])
        }
      }
      temp = JSON.stringify(temp)
      localStorage.setItem("Card",temp)
    },
    AddCardListData(cardList){
      let pub = []
      let temp = this.LoadCardListData()
      let idLists = this.LoadCardData("Card")
      for (let i=0;i<cardList.length;i++){
        if ((idLists.indexOf(cardList[i]['id'])===-1) && (pub.indexOf(cardList[i]['id'])===-1)){
          cardList[i]['get_time'] = new Date().getTime()
          cardList[i]['get_from_pool_name'] = this.selectPoolItem.meta.name
          cardList[i]['get_use_time'] = this.getHistory(this.selectPool)
          temp.push(cardList[i])
          pub.push(cardList[i]['id'])
        }
      }
      this.allMyCard = temp
      temp = JSON.stringify(temp)
      localStorage.setItem("mycardlist",temp)

    },
    sortPercentage(a,b){
      return b.p - a.p
    },
    loadInformation(){
      this.mostTimeData = []

      for(let i=0;i<5;i++){
        let mostList = []
        if (this.selectPoolItem['char'][i]!==null){
          this.selectPoolItem['char'][i].sort(this.sortPercentage);
          for (let j=0;j<this.selectPoolItem['char'][i].length;j++){
            mostList.push(this.selectPoolItem['char'][i][j]['p'])
          }

          this.mostTimeData[i] = parseFloat(this.getMostTime(mostList))

        }

      }
      this.open_frame = true

    },
    clear_data(){
      let clear = confirm("您确定要清除系统数据吗，您的所有卡牌将被删除，抽卡信息将重置")
      if (clear){
        this.allMyCard = []
        localStorage.clear()
        // localStorage.setItem('mycardlist',null)
        // localStorage.setItem('Card',null)
        alert("清除数据成功")
      }

    },
    skipFunction(){
        if (this.skipLock===false && this.skipResult===false){
          this.skipResult=true
          this.skipLock = true
        }

    },
    getMostTime(a){
      let obj = {} //采用键值对来存储，键表示该数字，值表示给数字出现次数
      let maxNum = 0
      a.forEach((item,index) => {
        if(a.indexOf(item) == index){
          obj[item] = 1
        }else{
          obj[item] = obj[item] + 1
        }
      })
//找出谁是最大值
      for(let i in obj){
        if(obj[i] > maxNum){
          maxNum = obj[i]
        }
      }
//根据最大值输出对应的数字
      for(let j in obj){
        if(obj[j] === maxNum){
          return j
        }
      }
    },
    LoadCardData(){
      let temp = localStorage.getItem("Card")
      if (temp===null || temp==="null"){
        temp = []
      }else{
        temp = JSON.parse(temp)
      }
      return temp


    },
    LoadCardListData(){
      let temp = localStorage.getItem("mycardlist")
      if (temp===null || temp==="null"){
        temp = []
      }else{
        temp = JSON.parse(temp)
      }
      return temp
    },
    LoadHistoryList(){
      let temp = localStorage.getItem("history")

      if (temp===null || temp==="null"){
        temp = {}
      }else{
        temp = JSON.parse(temp)
      }
      this.historyList = temp
    },
    getHistory(poolId){
      return this.historyList[poolId]
    },

    addHistory(poolId,times=1){
      if (!(poolId in this.historyList)){
        // console.log("dddd")
        this.historyList[poolId] = 0
      }

      this.historyList[poolId] += times
      let temp = JSON.stringify(this.historyList)
      localStorage.setItem("history",temp)
    },
    changePool(item){
      if (item.kira===true){
        if (this.kiraTimes[item.activity_id] === undefined){
          this.kiraTimes[item.activity_id] = 50
        }
      }
      this.poolKira = item.kira
      this.selectPoolItem = item
      this.selectPool = item.activity_id
      this.poolBackground = item.meta.img_src
    },
    requestFullScreen () {
      let de = document.documentElement
      if (de.requestFullscreen) {
        de.requestFullscreen()
      } else if (de.mozRequestFullScreen) {
        de.mozRequestFullScreen()
      } else if (de.webkitRequestFullScreen) {
        de.webkitRequestFullScreen()
      }
    },
    return_to_gacha_pool(){
      if (this.gacha_pool_music!==null){
        this.gacha_pool_music.play()
      }
      document.getElementById("gacha_stage").classList.add("hidden")
      this.removeHidden("#GachaPool")
      this.ResultLayer = false;
      this.loadingResource = true;
      this.start_document = false;
    },
    select_menu_func(id){
      this.select_menu = id
      document.getElementsByClassName("page")[0].scrollTop = 0
    },
    loading_gacha_resource(ten=true,cardPool_id=0){
      if (document.querySelector(".jump_in_stage")){
        document.querySelector(".jump_in_stage").classList.add("hidden")
      }
      this.ten_recruit =  ten

      if (ten){
        this.requireStar = 2500
        this.otherButtonText = "招募一次"

      }else{
        this.requireStar = 250
        this.otherButtonText = "招募十次"
      }
      // console.log("bug")
      if (this.nowStar-this.requireStar<0){
        this.caculateStar = this.requireStar - this.nowStar
        document.getElementById("no_enough_star").classList.remove("hidden")
        document.getElementById('hidebg').classList.remove('hidden')
        return;
      }else{
        document.getElementById("no_enough_star").classList.add("hidden")
        document.getElementById('hidebg').classList.add('hidden')
      }



      //读取卡牌信息 需后端
      let ten_r = 1
      if (this.ten_recruit===false){
        ten_r = 0
      }
      // let url = "http://127.0.0.1:5000/gacha/"
      let url = "https://api.bangdream.animedb.cn/gacha/"
      this.loading_system = true
      let keep = 0
      if (this.selectPoolItem.kira===true){
        if (ten_r===0){
          this.kiraTimes[this.selectPool] -= 1
        }else{
          this.kiraTimes[this.selectPool] -= 10
        }

        if (this.kiraTimes[this.selectPool]<=0){
          keep = 1
          this.kiraTimes[this.selectPool] = 50 - Math.abs(this.kiraTimes[this.selectPool])

        }
      }


      fetch(url+this.selectPool+"?ten="+ten_r+"&keep="+keep).then((response) => response.json()).then(
        (res)=>{
          if (res['status']===-1){

            this.system_tips = "通信故障"
            this.removeHidden("#hidebg")
            this.removeHidden("#system_tips")
            this.loading_system = false
            return;
          }
          // if (this.selectPoolItem.kira===true){
          //   if (ten_r===0){
          //     this.kiraTimes[this.selectPool] -= 1
          //   }else{
          //     this.kiraTimes[this.selectPool] -= 10
          //   }
          // }

          this.thisTimeCardList = res['data']
          // this.thisTimeCardList = this.cardStage

          this.cardStage = this.thisTimeCardList

          let id_arr = []
          for (let i=0;i<this.thisTimeCardList.length;i++){
            id_arr.push(this.thisTimeCardList[i].id)



            if (this.ifCardNew(this.thisTimeCardList[i].id)){
              this.thisTimeCardList[i]['new'] = true
              this.cardStage[i]['new'] = true
              this.cardList.push(this.thisTimeCardList[i].id)
              //console.log(i)
            }else{
              this.thisTimeCardList[i]['new'] = false
              this.cardStage[i]['new'] = false
            }
          }






          if (ten_r===0){
            this.addHistory(this.selectPool,1)
          }else{
            this.addHistory(this.selectPool,10)
          }

          this.AddCardListData(this.thisTimeCardList)
          this.AddCardData(id_arr)



          this.loading_system = false

          if (document.getElementById("gacha_stage")!==null){
            document.getElementById("gacha_stage").classList.add("hidden")
          }

          this.ResultLayer = false;
          this.addHidden("#GachaPool")
          this.cardStage = Object.assign([],this.thisTimeCardList)

          // this.loadingForGacha = true
          if (this.slogMusic===null){
            this.slogMusic = new Audio()
          }
          if (this.background_music===null){
            this.background_music = new Audio()
          }
          if (this.fadeInMusic===null){
            this.fadeInMusic = new Audio()
          }
          this.starMusicQue.push(new Audio())
          this.starMusicQue.push(new Audio())
          this.starMusicQue.push(new Audio())
          this.starMusicQue.push(new Audio())
          this.starMusicQue[0].src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3?a"
          this.starMusicQue[1].src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3?b"
          this.starMusicQue[3].src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3?c"
          this.starMusicQue[2].src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3?d"
          this.loadingForGacha = true
          this.saveNowStar(this.nowStar-this.requireStar)
          this.indexId = 0;
          // this.start_stage()



          let processAll = 0
          let successImg = 0
          this.preloadList = []

          // let process = 100/this.thisTimeCardList.length
          // console.log(process)
          for (let i=0;i<this.thisTimeCardList.length;i++){
            //废弃代码
            // let Img = new Image();
            // Img.onload = ()=>{
            //   successImg++;
            //   processAll+=process
            //   document.getElementById("loading_process").style.width=processAll+"%"
            //
            //   //全部读取完毕
            //   if (successImg>=this.thisTimeCardList.length){
            //     let needMusicForgacha = []
            //     for (let k=0;k<this.thisTimeCardList.length;k++){
            //       if (this.thisTimeCardList[k].star>=3){
            //         needMusicForgacha.push(this.thisTimeCardList[k].resource)
            //       }
            //     }
            //     let success_music = 0
            //     for (let j=0;j<needMusicForgacha.length;j++){
            //       let audio = new Audio()
            //       audio.oncanplay = ()=>{
            //         success_music+=1
            //         if (success_music===needMusicForgacha.length){
            //           setTimeout(()=>{
            //             this.loadingForGacha = false
            //             this.start_stage()
            //           },1500)
            //         }
            //       }
            //       audio.onerror = ()=>{
            //         success_music+=1
            //       }
            //       audio.src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/"+needMusicForgacha[j]+".mp3"
            //     }
            //   }
            // }
            // Img.src = this.thisTimeCardList[i].img_url
            // console.log(this.thisTimeCardList[i].direct_train)
            if (this.thisTimeCardList[i].star>=3){
              // this.preloadList.push(this.thisTimeCardList[i].img_url)
              let voiceType = this.getVoiceType(this.thisTimeCardList[i].id,this.thisTimeCardList[i].card_type)
              this.preloadList.push(`https://bestdori.com/assets/jp/sound/voice/gacha/${voiceType}_rip/${this.thisTimeCardList[i].resource}.mp3`)
              // if (this.thisTimeCardList[i].card_type!=="kirafes" && this.thisTimeCardList[i].card_type!=="birthday"){
              //   this.preloadList.push(`https://bestdori.com/assets/jp/characters/resourceset/${this.thisTimeCardList[i].resource}_rip/card_normal.png`)
              // }

              if (this.thisTimeCardList[i].direct_train===false){
                this.preloadList.push(`https://bestdori.com/assets/jp/characters/resourceset/${this.thisTimeCardList[i].resource}_rip/card_normal.png`)
              }else{
                this.preloadList.push(`https://bestdori.com/assets/jp/characters/resourceset/${this.thisTimeCardList[i].resource}_rip/card_after_training.png`)
              }
            }else{
              this.preloadList.push(`https://bestdori.com/assets/jp/characters/resourceset/${this.thisTimeCardList[i].resource}_rip/trim_normal.png`)

            }




            // this.preloadList.push(`https://koshigaya-komari.gitee.io/anime-reco/thumb2/${this.thisTimeCardList[i].resource}/${this.thisTimeCardList[i].resource}_${this.thisTimeCardList[i].direct_train===true?"after_training":"normal"}.png`)
            this.preloadList.push(`https://bestdori.com/assets/jp/thumb/chara/card${Math.floor(this.thisTimeCardList[i].id / 50).toString().padStart(5, "0")}_rip/${this.thisTimeCardList[i].resource}_${this.thisTimeCardList[i].direct_train===true?"after_training":"normal"}.png`)
          }
          let nowProcess = 0
          let count = this.preloadList.length
          let process = 100/count
          let que = new createjs.LoadQueue();
          que.setPreferXHR(false)
          que.on("fileload",()=>{
            nowProcess+=process
            // console.log(nowProcess)
            document.getElementById("loading_process").style.width=nowProcess+"%"
          })
          que.on("complete",()=>{
            setTimeout(()=>{
              this.loadingForGacha = false
              this.start_stage()
            },1500)
          })
          setTimeout(()=>{
            que.loadManifest(this.preloadList)
          },100)
        }
      )








    },
    start_test() {

      //播放背景音乐
      if (this.background_music===null){
        this.background_music = new Audio()
      }
      this.background_music.src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/gacha_bgm.mp3"
      this.background_music.play()
      this.background_music.loop = 999999
      // this.musicList.push(audio)
      //这里是显示卡片的地方
      this.start_document = true
      this.showCard(4, "上原 ひまり", "１人でがんばらせたりしない。 放っておいたりしないよ\n", "大丈夫じゃないなら", "https://oosaka.cdn.animedb.cn/BDP/test/img.png", "res004063")
      // this.start_document = false
    },
      // this.showCard(4,"山吹沙绫","このまま、\nずっとみんなで演奏したいって","全力文化祭","https://oosaka.cdn.animedb.cn/BDP/card_normal.png","res004063")
      // this.start_document = false
    //   setTimeout(()=>{
    //
    //
    // },
    start_stage(){
      this.skipLock = false
      this.skipResult=false
      this.ticketLeft = "https://oosaka.cdn.animedb.cn/BDP/tex_tiket_normal_L.png"
      this.ticketRight = "https://oosaka.cdn.animedb.cn/BDP/tex_tiket_normal_R.png"
      let best_star = 0
      for (let i=0;i<this.cardStage.length;i++){
        if (this.cardStage[i].star===5){
          this.ticketLeft = "https://oosaka.cdn.animedb.cn/BDP/tex_tiket_star5_L.png"
          this.ticketRight = "https://oosaka.cdn.animedb.cn/BDP/tex_tiket_star5_R.png"
        }
        if (best_star<this.cardStage[i].star){
          best_star = this.cardStage[i].star

        }
      }

      if (this.gacha_pool_music!==null){
        this.gacha_pool_music.pause()
        this.gacha_pool_music.currentTime = 0
      }
      document.getElementById("tickets").classList.add("tickets_fadeIn")
      //播放背景音乐

      this.background_music.src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/gacha_bgm.mp3"
      this.background_music.play()

      this.background_music.loop = 999999

      document.querySelector("#gacha_stage").classList.remove("hidden")
      document.getElementById("tickets").classList.remove("hidden")
      this.createCustomers(best_star,true)
      setTimeout(()=>{
        document.getElementById("ticket_open_button").classList.remove("hidden")
        setTimeout(()=>{
          document.getElementById("ticket_open_button").innerText = "TOUCH TO CUT"

        },300)
        document.getElementById("tickets").addEventListener("click",()=>{

          document.getElementById("blackMaskForStage").classList.remove("hidden")
          document.getElementById("blackMaskForStage").classList.add("blackMaskForStageAnimate_css")
          document.getElementById("tickets").classList.remove("tickets_fadeIn")
          document.getElementById("tickets").classList.add("broken_ticket")


          setTimeout(()=>{
            document.getElementById("ticket_open_button").classList.add("hidden")
            document.querySelector(".tex_ticket_left").classList.add("broken_ticket_left")
            document.querySelector(".tex_ticket_right").classList.add("broken_ticket_right")

          },600)
          setTimeout(()=>{
            document.getElementById("ticket_open_button").innerText = ""
            document.querySelector(".jump_in_stage").classList.remove("hidden")
            document.getElementById("tickets").classList.remove("broken_ticket")

            document.getElementById("jump_in_stage").classList.remove("jump_in_stage")

          },700)
          setTimeout(()=>{
            document.getElementById("jump_in_stage").classList.add("jump_in_stage")
            document.getElementById("jump_in_stage").classList.remove("no_light")
          },900)
          setTimeout(()=>{
            document.querySelector(".tex_ticket_left").classList.remove("broken_ticket_left")
            document.querySelector(".tex_ticket_right").classList.remove("broken_ticket_right")
            document.getElementById("tickets").classList.add("hidden")
          },1500)
          setTimeout(()=>{

            document.getElementById("blackMaskForStage").classList.add("hidden")
            // document.getElementById("gacha_stage").classList.add("hidden")
            document.getElementById("blackMaskForStage").classList.remove("blackMaskForStageAnimate_css")

            // setTimeout(()=>{
            //   this.showCard(5, "上原 ひまり", "１人でがんばらせたりしない。 放っておいたりしないよ\n", "大丈夫じゃないなら", "https://oosaka.cdn.animedb.cn/BDP/test/img.png", "res004063")
            //
            // },17000)
            // this.showCard(5, "上原 ひまり", "１人でがんばらせたりしない。 放っておいたりしないよ\n", "大丈夫じゃないなら", "https://oosaka.cdn.animedb.cn/BDP/test/img.png", "res004063")
            this.skipButton = true
            this.inStageprocess()
          },400)
        }, { once: true })
      },1500)



    },
    inStageprocess(){

      //1500
      this.clearALlTimeOut()
      // this.start_document = false
      document.getElementById("gacha_stage").classList.remove("hidden")
      // console.log(this.cardStage[0])

      if (this.cardStage[0]===undefined){
        this.skipLock = true
        this.createCustomers(2)
        this.gacha_stage_background = "https://oosaka.cdn.animedb.cn/BDP/other_star_stage.png"

        setTimeout(()=>{
          //关闭document
          this.start_document = false;

          this.background_music.pause();
          this.slogMusic.pause();
          this.removeCustomers();

          this.skipButton = false;
          this.ResultLayer = true;

          document.getElementById("no_enough_star").classList.add("hidden")
        },300)
        return;
      }else{
        if (this.skipResult){
          this.skipLock = true
          let skipFinish = false
          while (this.cardStage.length>1 && skipFinish===false){
            //console.log(this.cardStage[0])
            if (this.cardStage[0].star<=2 || this.cardStage[0]['new']===false){
              this.indexId += 1
              this.cardStage.shift()
            }else{
              skipFinish = true
            }
          }
        }
        if (this.cardStage[0].star<=2 && this.skipResult===true && this.cardStage.length===1){
          this.cardStage.shift()
          this.inStageprocess()
          return;
        }

        this.createCustomers(this.cardStage[0].star)
        //这里是舞台的效果
        if (this.cardStage[0].star===4){
          this.gacha_stage_background = "https://oosaka.cdn.animedb.cn/BDP/four_star_stage.png"
        }else if (this.cardStage[0].star===3){
          this.gacha_stage_background = "https://oosaka.cdn.animedb.cn/BDP/three_star_stage.png"
        }else if (this.cardStage[0].star===5){
          this.gacha_stage_background = "https://oosaka.cdn.animedb.cn/BDP/five_star_stage.png"
        }else{
          this.gacha_stage_background = "https://oosaka.cdn.animedb.cn/BDP/other_star_stage.png"
        }
      }

      this.rth_music.loop = false
      this.rth_music.currentTime = 0
      this.rth_music.play()

        //这个是开启抽卡页面的
        setTimeout(()=>{
          let cardi = this.cardStage[0]
          this.cardStage.shift()
          this.showCard(cardi.star,cardi.name,cardi.slog,cardi.title,cardi.img_url,cardi.resource,cardi.type,cardi.id,cardi.cid,cardi.card_type,cardi.direct_train)

        },1500)
    },
    clearMusicList(){
      for (let i=0;i<this.musicList.length;i++){
        this.musicList[i].pause()
      }
    },
    clearALlTimeOut(){
      for (let i=0;i<this.taskId.length;i++){
        clearTimeout(this.taskId[i])
      }
    },
    showCard(star_num,character_name,slog,title,img_src,resourcepath,type,id,cid,card_type,direct_train){
      let abortMess = new AbortController()
      let {signal} = abortMess;
      if (star_num<=2 && this.skipResult){
        this.inStageprocess()
        return;
      }else{
        this.skipLock = true
      }
      this.indexId += 1
      this.card_img = img_src
      this.card_character = this.getCharacterNameAndBand(cid)[0]
      this.card_slogen = slog
      this.card_title = title
      this.card_resourceId = resourcepath
      this.card_type = type

      this.card_star = star_num
      if (star_num<=2){
        this.card_img = `https://bestdori.com/assets/jp/characters/resourceset/${resourcepath}_rip/trim_normal.png` //如果是1-2星没有所谓的卡图
      }else{
        if (direct_train===false){
          this.card_img = `https://bestdori.com/assets/jp/characters/resourceset/${resourcepath}_rip/card_normal.png`
          this.trainStar = ""
        }else{
          this.card_img = `https://bestdori.com/assets/jp/characters/resourceset/${resourcepath}_rip/card_after_training.png`
          this.trainStar = "train_stars"
        }

      }
      let starList = document.querySelectorAll('.stars');
      for(let i=0;i<starList.length;i++){
        starList[i].classList.add("hidden")
      }
      if (document.getElementById("showCard")!==null){

        document.getElementById("maskForRecruit").classList.add("hidden")
        document.getElementById("maskForRecruit").classList.remove("bottom_recruit_slog")
      }
      if (document.querySelector(".three_star")!==null){
        document.getElementById("three_star_pos1").classList.add("hidden")
        document.getElementById("three_star_pos2").classList.add("hidden")
      }

      if (document.getElementById("showCardFive")!==null){
        document.querySelector(".five-stars").classList.add("hidden")
        document.querySelector("#blackMaskButtom").classList.remove("hidden")
        document.querySelector("#blackMaskTop").classList.remove("hidden")
        document.querySelector(".recruit_slog").classList.remove("hidden")
        document.getElementById("blackMaskTop").classList.remove("fadeoutfivecard")
        document.getElementById("blackMaskButtom").classList.remove("fadeoutfivecardbottom")
      }
      if (document.getElementById("cardLayer")!==null){
        document.getElementById("cardLayer").classList.add("hidden")

        document.getElementById("card_img").classList.remove("card_img_animate_for_five")
        document.getElementById("card_information").classList.add("hidden")
        document.getElementById("card_img").classList.remove("card_in_animate")
      }


      if (document.querySelector(".recruit_slog_fadein")!==null){
        document.querySelector(".recruit_slog_fadein").classList.remove("recruit_slog_fadein_animate")
      }

      this.start_document = false
      this.clearALlTimeOut()
      // console.log(star_num)
      //二星一星的处理

      if (star_num<=2){

        // let star_music = new Audio("https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3")
        this.portal_img = img_src
        document.querySelector(".three_star").classList.remove("hidden")
        document.getElementById("jump_in_stage").classList.remove("jump_in_stage")
        this.taskId.push(setTimeout(()=>{
          if (this.thisTimeCardList[this.indexId-1]['new']===true){
            document.getElementById("card_new_stage").classList.remove("hidden")
          }
        },600))
        this.taskId.push(setTimeout(()=>{
          let e = this
          for (let i=1;i<=star_num;i++){
            (function (ti){
              var timeout
              if (ti===5){
                timeout = (ti*250)+400
              }else{
                timeout = ti*250
              }
              e.taskId.push(setTimeout(()=>{
                if (e.starMusicQue[0].paused){
                  e.starMusicQue[0].play()
                }else if (e.starMusicQue[1].paused){
                  e.starMusicQue[1].play()
                }else if (e.starMusicQue[2].paused){
                  e.starMusicQue[2].play()
                }
                document.getElementById("three_star_pos"+ti).classList.remove("hidden")
              },timeout))
            })(i)
          }
        },200))
        setTimeout(()=>{
          let ae = this
          function fn(){
            abortMess.abort()
            document.getElementById("jump_in_stage").classList.add("jump_in_stage")
            document.querySelector(".three_star").classList.add("hidden")
            document.getElementById("card_new_stage").classList.add("hidden")
            ae.inStageprocess()
          }

          document.getElementById("gacha_stage").addEventListener("click",()=>{
            this.skipResult = false
            this.skipLock = false

            fn()
          },{once:true,signal})

          document.getElementById("skipButton").addEventListener("click",()=>{
            this.skipResult = true
            this.skipLock = false
            if (this.skipLock===false && this.skipResult===false){
              this.skipResult=true
              this.skipLock = true
            }
            fn()

          },{once:true,signal})

        },800)
        return;
      }else{
        document.getElementById("gacha_stage").classList.add("hidden")
      }

      this.start_document = true

      var id;
      // alert(resourcepath)





      // this.cardLayer = false
      if (this.card_star>=3 && this.card_star<=4){
        // this.maskOpenForOther = true
        setTimeout(()=>{
          if (document.querySelector(".recruit_slog_for_not_five")!==null){
            document.querySelector(".recruit_slog_for_not_five").classList.remove("hidden")
          }
        },100)
        if (document.querySelector(".recruit_slog_fadein")!==null){
          document.querySelector(".recruit_slog_fadein").classList.remove("recruit_slog_fadein_animate")
          document.querySelector(".recruit_slog_fadein").classList.add("recruit_slog_fadein_animate")


        }

        if (document.querySelector(".recruit_slog_for_not_five")!==null){

          document.querySelector(".recruit_slog_for_not_five").classList.remove("hidden")
        }
        if (document.getElementById("blackMaskTop")!==null){
          document.getElementById("blackMaskTop").classList.remove("hidden")
        }
        if (document.getElementById("blackMaskButtom")!==null){
          document.getElementById("blackMaskButtom").classList.remove("hidden")
        }
        if (document.getElementById("recruit_slog")!==null){
          document.getElementById("recruit_slog").classList.remove("hidden")

        }
        if (document.getElementById("blackMask")!==null){
          document.getElementById("blackMask").classList.remove("hidden")
        }

      }

      document.getElementById("blackMaskForStage").classList.add("hidden")
      if (this.card_slogen!==null){
        let voiceType = this.getVoiceType(id,card_type)

          // this.slogMusic.src = "https://koshigaya-komari.gitee.io/anime-reco/gacha_music/"+resourcepath+".mp3"
        this.slogMusic.src = `https://bestdori.com/assets/jp/sound/voice/gacha/${voiceType}_rip/${resourcepath}.mp3`

        document.getElementById("skipButton").addEventListener("click",()=>{
          abortMess.abort()
          this.showCardLayer(img_src,star_num,resourcepath,id)

        },{once:true,signal})

          this.slogMusic.play().then(()=>{
            this.slogMusic.addEventListener("ended",()=>{
              abortMess.abort()
              setTimeout(()=>{
                this.showCardLayer(img_src,star_num,resourcepath,id)
              },500)
            },{once:true,signal})
          }).catch(()=>{
            abortMess.abort()
            setTimeout(()=>{
              this.showCardLayer(img_src,star_num,resourcepath,id)
            },500)
          })
      }else{
        setTimeout(()=>{
          this.showCardLayer(img_src,star_num,resourcepath,id)
        },500)
      }

    },
    showCardLayer(img_url,star_num,rp,id){
      let abortMess = new AbortController()
      let {signal} = abortMess;
      // console.log(this.indexId)
      document.getElementById("cardLayer").classList.remove("hidden")
      var id;
      switch (star_num){
        case 4:
          this.card_information_background = "https://oosaka.cdn.animedb.cn/BDP/card/four_star_background.png"
          break
        case 5:
          this.card_information_background = "https://oosaka.cdn.animedb.cn/BDP/card/five_star_background.png"
          break
        case 3:
          this.card_information_background = "https://oosaka.cdn.animedb.cn/BDP/card/three_star_background.png"
          break
      }
      this.cardLayer = true
      if (star_num>=3 && star_num<=4){
        document.getElementById("maskForRecruit").classList.add("bottom_recruit_slog")
        document.getElementById("maskForRecruit").classList.remove("hidden")
        document.getElementById("card_img").classList.add("card_in_animate")
        this.fadeInMusic.src = "https://oosaka.cdn.animedb.cn/BDP_MUSIC/fadeIn.mp3"
        this.fadeInMusic.volume = 1
        this.fadeInMusic.play()
        // let fadeinMusic = new Audio("https://oosaka.cdn.animedb.cn/BDP_MUSIC/fadeIn.mp3")
        // fadeinMusic.volume = 1
        // fadeinMusic.play()

        //去除掉招募slog
        this.taskId.push(setTimeout(()=>{

          document.getElementById("blackMask").classList.add("hidden")
          document.getElementById("recruit_slog").classList.add("hidden")
          document.querySelector(".recruit_slog_fadein").classList.remove("recruit_slog_fadein_animate")
          if (document.querySelector(".recruit_slog_for_not_five")!==null){
            document.querySelector(".recruit_slog_for_not_five").classList.add("hidden")
          }
        },1500))
      }

      if (this.card_star===5){

        document.getElementById("blackMaskTop").classList.add("fadeoutfivecard")
        document.getElementById("blackMaskButtom").classList.add("fadeoutfivecardbottom")
        document.getElementById("card_img").classList.add("card_img_animate_for_five")

        this.taskId.push(setTimeout(()=>{
          document.getElementById("recruit_slog").classList.add("hidden")
        },300))
        this.taskId.push(setTimeout(()=>{
          document.getElementById("blackMaskTop").classList.add("hidden")
          document.getElementById("blackMaskButtom").classList.add("hidden")
        },7300))
        this.taskId.push(setTimeout(()=>{
          document.getElementById("card_img_shadow").classList.remove("hidden")
        },7110))
        this.taskId.push(setTimeout(()=>{
          document.getElementById("card_img_shadow").classList.add("hidden")
        },7500))


      }
      this.taskId.push(setTimeout(()=>{
        if (this.thisTimeCardList[this.indexId-1]['new']===true){
          document.getElementById("card_information_new").classList.remove("hidden")
        }

      },4000))
      //this input

      // setTimeout(()=>{
      //   document.getElementById("maskForRecruit").classList.add("hidden")
      // },3000)
      //显示information
      let delay = 0
      if (star_num===5){
        delay = 9000
      }else{
        delay = 3000
      }
      this.taskId.push(setTimeout(()=>{

        document.getElementById("card_information").classList.remove("hidden")
      },delay))
      let star_music = new Audio("https://oosaka.cdn.animedb.cn/BDP_MUSIC/stars.mp3")

      this.starMusicQue[0].currentTime = 0
      this.starMusicQue[1].currentTime = 0
      this.starMusicQue[2].currentTime = 0
      this.starMusicQue[3].currentTime = 0
      //这里是判断星星数量显示的
      this.taskId.push(setTimeout(()=>{
        let e = this
        for (let i=1;i<=star_num;i++){
          (function (ti){
            var timeout
            if (ti===5){
              timeout = (ti*250)+400
            }else{
              timeout = ti*250
            }
            e.taskId.push(setTimeout(()=>{

              if (e.starMusicQue[0].paused){

                e.starMusicQue[0].play()
              }else if (e.starMusicQue[1].paused){
                e.starMusicQue[1].play()
              }else if (e.starMusicQue[2].paused){
                e.starMusicQue[2].play()
              }else if (e.starMusicQue[3].paused){

                e.starMusicQue[3].play()
              }
              if (star_num===i){
                let id = "showCard"
                if (star_num===5){
                  id = "showCardFive"
                }
                function fn(){
                  abortMess.abort()
                  e.inStageprocess()
                  document.getElementById("card_information_new").classList.add("hidden")
                  for (let i=1;i<=5;i++){
                    document.getElementById("star_pos"+ti).classList.add("hidden")
                  }
                }
                document.getElementById(id).addEventListener("click",()=>{
                  e.skipLock = false
                  e.skipResult = false
                  fn()


                },{once:true,signal})


                document.getElementById("skipButton").addEventListener("click",()=>{
                  e.skipLock = false
                  e.skipResult = true
                  e.skipFunction()
                  fn()
                },{once:true,signal})
              }
              document.getElementById("star_pos"+ti).classList.remove("hidden")
            },timeout))
          })(i)
        }
      },delay+1000))

      // this.changeBackground(img_url)
    },
    changeBackground(url){
      window.document.body.style.backgroundImage ="url('"+url+"')"
    },
    changeBackgroundColor(color){
      // window.document.body.style.backgroundImage =""
      window.document.body.style.background = color
    },
    changeLight(type="common"){
      switch (type) {
        case "gold":
          break
        case "color":

          break
        default:
          break
      }
    },
    showTicket(){
      return
    },
    getVoiceType(id,card_type){
      let voiceType = "spin"
      if (1353 <= id && id < 5e3){
        if (card_type==="birthday"){
          voiceType = "birthdayspin"
        }else if (card_type==="permanent"){
          voiceType = "operationspin"
        }else{
          voiceType = "limitedspin"
        }
      }
      return voiceType
    }
  }

}
</script>
