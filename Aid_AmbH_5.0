// ==UserScript==
// @name        Aid AmbH
// @namespace        http://tampermonkey.net/
// @version        5.0
// @description        「HOME」「ブログ」のリンク動作を改善
// @author        Ameba blog User
// @match        https://ameblo.jp/*
// @match        https://www.ameba.jp/home
// @match        https://www.ameba.jp/notifications
// @match        https://blog.ameba.jp/ucs/comment/commentlist*
// @match        https://blog.ameba.jp/ucs/iine/*
// @match        https://msg.ameba.jp/*
// @icon        https://www.google.com/s2/favicons?sz=64&domain=ameblo.jp
// @noframes
// @grant        none
// @updateURL        https://github.com/personwritep/Aid_AmbH/raw/main/Aid_AmbH.user.js
// @downloadURL        https://github.com/personwritep/Aid_AmbH/raw/main/Aid_AmbH.user.js
// ==/UserScript==


if(location.hostname=='ameblo.jp'){ // 通常のブログページ
    let once=0; // 表示操作の複数回動作を抑止するフラグ

    let target0=document.querySelector('head');
    let monitor0=new MutationObserver(main);
    monitor0.observe(target0, { childList: true });

    main();

    function main(){

        let ua=0;
        let agent=window.navigator.userAgent.toLowerCase();
        if(agent.indexOf('firefox') > -1){ ua=1; } // Firefoxの場合のフラッグ

        let skin_type=0;
        let skin_code=document.documentElement.getAttribute('data-base-skin-code');
        switch(skin_code){
            case "uranus": skin_type=0; break;
            case "new": skin_type=1; break;
            case "default": skin_type=2; break;
            default: skin_type=0; }


        //「HOME」「コメント管理」からコメント欄に直行の機能を使った場合の処理
        if(location.hash=='#cbox'){ // #cbox付きURLで開かれた場合
            if(once==0){
                let main=document.querySelector('#main');
                if(main){
                    main.scrollIntoView({ block: "end" }); } // 本文末尾へスクロール

                let retry=0;
                let interval=setInterval(wait_target, 100);
                function wait_target(){
                    retry++;
                    if(retry>100){ // リトライ制限 20回 2sec
                        clearInterval(interval);
                        once=1;
                        aid_comm_remove(); } //「コメント欄」が無い場合は操作を停止 🔵
                    let target=document.querySelector('#cbox'); // 監視 target
                    if(target){
                        clearInterval(interval);
                        once=1; // コメント欄の取得
                        com_disp(target); }}} // 独自パネルを表示 🔵

        } // #cbox付きURLで開かれた場合


        //「コメントを見る」を押した場合の処理
        let sw=document.querySelector('.commentLink');
        if(sw){
            sw.addEventListener('click', function(event){
                let cbox=document.querySelector('#cbox');
                if(cbox){
                    com_disp(cbox); } // 独自パネルを表示 🔵
                else{
                    sw.click(); }}); } // コメント欄の無いページからデフォルトのリンク移動


        //「コメント欄のヘッダー」を押した場合の処理
        let comm_header;
        if(skin_type==0){
            comm_header=document.querySelector('#commentsHeader h4'); }
        else if(skin_type==1){
            comm_header=document.querySelector('h1.commentTitle'); }
        else if(skin_type==2){
            comm_header=document.querySelector('#comment_module h3.title'); }
        if(comm_header){
            comm_header.addEventListener('click', function(event){
                if(!event.ctrlKey){
                    let cbox=document.querySelector('#cbox');
                    if(cbox){
                        com_disp(cbox); }}}); } // 独自パネルを表示 🔵


        let commWrapp=document.querySelector('.comm_wrapp');
        if(!commWrapp){
            aid_comm_remove(); } // ページ移動があった場合にリセット 🔵



        function aid_env(){
            let ac_style=
                '<style class="aid_comm">'+
                'html { scroll-behavior: unset; overflow-y: hidden; } '+
                'body { zoom: unset !important; '+
                'transform: unset !important; width: unset !important; } '+
                '.comm_wrapp { '+
                'display: flex; flex-direction: column; '+
                'position: fixed !important; z-index: 1999 !important; max-height: 80vh; '+
                'top: 80px; left: calc(50% - 390px); width: 712px; margin: 0; '+
                'padding:20px 25px 20px 40px !important; color: #000; background: #fff; '+
                'border: 1px solid #aaa; box-shadow: 0 0 0 100vw rgb(0 0 0 / 25%); } '+

                '.comm_wrapp *:not([data-uranus-icon]) { '+
                'color: #000 !important; } '+

                'button.commentWinOpenBtn { '+
                'font: 16px Meiryo !important; padding-top: 2px; '+
                'position: absolute; top: 20px; left: 300px; height: 28px; z-index: 1; '+
                'border: 1px solid #777; border-radius: 3px; background: #eceff1; '+
                'display: flex; align-items: center; justify-content: center; } '+

                '.comm_wrapp .color_sw, .comm_wrapp .color_box, .comm_wrapp .clear_reset { '+
                'font: 16px Meiryo !important; padding: 1px 0; '+
                'position: absolute; top: 21px; height: 27px; width: 27px; cursor: pointer; } '+
                '.comm_wrapp .color_sw, .comm_wrapp .color_box { left: 670px; } '+
                '.comm_wrapp .color_sw { z-index: 2; } '+
                '.comm_wrapp .color_box { z-index: 1; } '+
                '.comm_wrapp .clear_reset { left: 710px; padding: 0 5px; z-index: 1; } '+

                '[data-uranus-component="mainWidgetBody"], '+
                '#comment_module, #commentListUl { '+
                'overflow-y: scroll; overflow-x: hidden; overscroll-behavior-y: contain; padding: 0 0 1px; } '+
                '[data-uranus-component="mainWidgetBody"]::-webkit-scrollbar, '+
                '#comment_module::-webkit-scrollbar, '+
                '#commentListUl::-webkit-scrollbar { width: 15px; } '+
                '[data-uranus-component="mainWidgetBody"]::-webkit-scrollbar-thumb, '+
                '#comment_module::-webkit-scrollbar-thumb, '+
                '#commentListUl::-webkit-scrollbar-thumb { '+
                'background: #ddd; box-shadow: inset 10px 0 0 0 #fff; } '+
                '[data-uranus-component="mainWidgetBody"]::-webkit-scrollbar-thumb:hover, '+
                '#comment_module::-webkit-scrollbar-thumb:hover, '+
                '#commentListUl::-webkit-scrollbar-thumb:hover { '+
                'background: #ddd; box-shadow: inset 2px 0 0 0 #fff; } '+
                '[data-uranus-component="mainWidgetBody"]::-webkit-scrollbar-track, '+
                '#comment_module::-webkit-scrollbar-track, '+
                '#commentListUl::-webkit-scrollbar-track { background: transparent; } ';

            if(ua==1){
                ac_style+=
                    '[data-uranus-component="mainWidgetBody"], '+
                    '#comment_module, #commentListUl { '+
                    'padding-right: 15px; scrollbar-color: #aaa transparent; } '; }

            if(skin_type==0){ // 新タイプスキン
                ac_style+=
                    '.skin-borderLoud, .skin-borderQuiet { border: none !important; } '+ // border 🔴
                    '[data-uranus-component="commentsList"] >li { '+ // border 🔴
                    'border-bottom: 1px dotted #888; } '+ // border 🔴
                    '#commentsHeader { padding-bottom: 20px; margin: 0 15px 0 0; } '+
                    '#commentsHeader h4 { font: 16px Meiryo !important; } '+
                    '#commentsList { margin: 0; } '+
                    '[data-uranus-component="mainWidgetFooter"] { padding: 0; } '+
                    '[data-uranus-icon~="hasLabel"] { font-family: "AmebaNewSymbols" !important; } '+
                    '</style>'; }

            if(skin_type==1){ // 旧タイプスキン
                ac_style+=
                    '.skinBorderHr, .skinBorderList li { border-color: #888; } '+
                    '.skinBorderHr { flex-shrink: 0; } '+
                    '.commentOpenArea.skinWeakColor { display: none; } '+
                    '.commentTitleArea.skinBorderHr { margin: 0 15px 0 0; } '+
                    'h1.commentTitle { font: 16px Meiryo !important; margin: 0 15px 16px 0; } '+
                    '.commentBtnArea { padding: 0; } '+
                    '</style>'; }

            if(skin_type==2){ //レトロタイプスキン
                ac_style+=
                    '#comment_module p.list { display: none; } '+
                    '#comment_module h3.title { font: 16px Meiryo !important; } '+
                    '#comment_module .each_comment { margin: 0; padding: 20px 0; } '+
                    '</style>'; }

            if(!document.querySelector('.aid_comm')){
                document.documentElement.insertAdjacentHTML('beforeend', ac_style); }

        } // aid_env()



        function com_disp(cbox){
            aid_env();


            let commMore=document.querySelectorAll(
                '[data-uranus-component="commentsMoreButton"], #commentListMoreLink')[0];
            if(commMore){
                commMore.click(); } // コメント欄の全展開　新・旧タイプスキン共用


            if(skin_type==0){ // 新タイプスキン
                let cbox_wrapp=cbox.closest('[data-uranus-component="mainWidget"]');
                cbox_wrapp.classList.add('comm_wrapp'); }
            else if(skin_type==1){ // 旧タイプスキン
                document.querySelector('.commentArea').classList.add('comm_wrapp'); }
            else if(skin_type==2){ // レトロタイプスキン
                document.querySelector('#comment_module').classList.add('comm_wrapp'); }


            let commWrapp=document.querySelector('.comm_wrapp');
            if(commWrapp){
                if(!commWrapp.querySelector('.aid_comm_con')){
                    let control=
                        '<div class="aid_comm_con">'+
                        '<input type="button" class="clear_reset" value="✖">'+
                        '<input type="button" class="color_sw" value="" '+
                        'title="Click: 背景色を指定\nCtrl+Click: 背景色をリセット\nWheel: 文字サイズ">'+
                        '<input type="color" class="color_box"></div>';
                    commWrapp.insertAdjacentHTML('beforeend', control); }



                let color_sw=commWrapp.querySelector('.color_sw');
                if(color_sw){
                    let sw_col=localStorage.getItem('aid_ambh_sw_col'); // ストレージから取得
                    if(sw_col==null){
                        sw_col='transparent'; // デフォルト背景色
                        localStorage.setItem('aid_ambh_sw_col', sw_col); }
                    set_color(sw_col);

                    color_sw.onclick=function(event){
                        if(!event.ctrlKey){
                            let color_box=commWrapp.querySelector('.color_box');
                            color_box.value=sw_col;
                            color_box.click();
                            color_box.oninput=function(){
                                set_color(color_box.value);
                                localStorage.setItem('aid_ambh_sw_col', color_box.value); }}
                        else if(event.ctrlKey){
                            let ok=confirm("ブログごとのコメント背景色を適用します");
                            if(ok){
                                sw_col='transparent'; // 透明
                                set_color(sw_col);
                                localStorage.setItem('aid_ambh_sw_col', sw_col); }}}

                    function set_color(col){
                        let color_style;
                        if(col!='transparent'){
                            if(skin_type==0){ // 新タイプスキン
                                color_style='<style class="aid_comm_col">'+
                                    '.skin-bgQuiet { background: '+ col +'; } '+
                                    '.comm_wrapp .color_sw { box-shadow: inset 0 0 0 20px '+ col +'; }'+
                                    '</style>'; }
                            else if(skin_type==1){ // 旧タイプスキン
                                color_style='<style class="aid_comm_col">'+
                                    '.skinStrongBgColor { background: '+ col +'; } '+
                                    '.comm_wrapp .color_sw { box-shadow: inset 0 0 0 20px '+ col +'; }'+
                                    '</style>' }
                            if(document.querySelector('.aid_comm_col')){
                                document.querySelector('.aid_comm_col').remove(); }
                            document.body.insertAdjacentHTML('beforeend', color_style); }}



                    let fontsz=localStorage.getItem('aid_ambh_fontsz'); // ストレージから取得
                    if(fontsz==null || fontsz>20 || fontsz<13){
                        fontsz='16'; // デフォルトフォントサイズ
                        localStorage.setItem('aid_ambh_fontsz', fontsz); }
                    set_fontsz(fontsz);

                    color_sw.onwheel=function(event){ // マスウホイールで設定
                        let color_sw=document.querySelector('.color_sw');
                        if(color_sw){
                            if(event.deltaY<0 && fontsz<20){
                                event.preventDefault();
                                event.stopImmediatePropagation();
                                fontsz=fontsz/1 +1;
                                color_sw.value=fontsz;
                                localStorage.setItem('aid_ambh_fontsz', fontsz); }
                            else if(event.deltaY>0 && fontsz>13){
                                event.preventDefault();
                                event.stopImmediatePropagation();
                                fontsz=fontsz/1 -1;
                                color_sw.value=fontsz;
                                localStorage.setItem('aid_ambh_fontsz', fontsz); }
                            set_fontsz(fontsz); }}

                    function set_fontsz(fontsz){
                        color_sw.value=fontsz;
                        let sz_style=
                            '<style class="aid_comm_sz">'+
                            '#commentsList *, #commentListUl *, .each_comment * { font: '+
                            fontsz +'px Meiryo; }'+
                            '<style>';
                        if(document.querySelector('.aid_comm_sz')){
                            document.querySelector('.aid_comm_sz').remove(); }
                        document.documentElement.insertAdjacentHTML('beforeend', sz_style); }

                } // if(color_sw)



                let clear_reset=commWrapp.querySelector('.clear_reset');
                if(clear_reset){
                    clear_reset.onclick=function(event){
                        event.stopImmediatePropagation();
                        aid_comm_remove();
                        history.pushState('0', '0', location.href.replace(/#cbox/, '')); }} //「#cbox」削除

            } // if(commWrapp)
        } // com_disp()



        function aid_comm_remove(){
            if(document.querySelector('.aid_comm_con')){
                document.querySelector('.aid_comm_con').remove(); }

            if(document.querySelector('.aid_comm')){
                document.querySelector('.aid_comm').remove(); }

            if(document.querySelector('.aid_comm_col')){
                document.querySelector('.aid_comm_col').remove(); }

            if(document.querySelector('.aid_comm_sz')){
                document.querySelector('.aid_comm_sz').remove(); }}




        // アメブロヘッダーのアイコン表示
        let n0=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23009688'>";
        let h0=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23ffffff'>";
        let b0=
            "<g transform='translate(0,64) scale(0.1,-0.1)'>"+
            "<path d='M212 558 c-7 -7 -12 -35 -12 -64 0 -28 -7 -68 -15 -87 -8 -20 -15 -6"+
            "6 -15 -104 0 -61 -3 -69 -27 -89 -32 -25 -27 -60 8 -49 14 5 25 2 30 -6 8 -11"+
            " 12 -10 24 5 15 20 15 20 15 -1 0 -11 -9 -28 -20 -38 -31 -28 -8 -47 54 -43 4"+
            "4 3 51 6 54 25 4 30 20 30 24 1 3 -20 10 -23 54 -26 62 -4 85 15 54 43 -11 10"+
            " -20 27 -20 38 0 21 0 21 15 1 12 -15 16 -16 24 -5 5 8 16 11 30 6 35 -11 40 "+
            "24 8 49 -25 20 -27 28 -27 91 -1 39 -7 88 -15 110 -8 22 -15 61 -15 86 0 30 -"+
            "6 51 -15 59 -13 11 -21 8 -48 -18 -42 -41 -75 -41 -112 -2 -32 34 -36 35 -53 "+
            "18z m38 -148 c0 -5 -4 -10 -10 -10 -5 0 -10 5 -10 10 0 6 5 10 10 10 6 0 10 -"+
            "4 10 -10z m150 0 c0 -5 -4 -10 -10 -10 -5 0 -10 5 -10 10 0 6 5 10 10 10 6 0 "+
            "10 -4 10 -10z m-103 -28 c-19 -20 -7 -42 23 -42 30 0 42 22 23 42 -16 18 -16 "+
            "18 3 12 26 -9 64 -45 64 -60 0 -6 -7 -18 -16 -25 -14 -11 -21 -11 -45 3 -27 1"+
            "6 -31 16 -58 0 -24 -14 -31 -14 -45 -3 -9 7 -16 19 -16 25 0 20 48 59 79 65 2"+
            " 1 -3 -7 -12 -17z m61 -128 c33 -23 27 -73 -10 -94 -25 -13 -31 -13 -55 0 -38"+
            " 21 -44 71 -11 94 12 9 29 16 38 16 9 0 26 -7 38 -16z'/></g></svg>";
        let icon0n=n0+b0;
        let icon0h=h0+b0;

        let n1=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23ff9800'>";
        let h1=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23ffffff'>";
        let b1=
            "<g transform='translate(0,64) scale(0.1,-0.1)'>"+
            "<path d='M255 506 c-74 -33 -95 -88 -95 -256 0 -117 2 -132 20 -150 24 -24 48"+
            " -25 78 -4 17 12 22 25 22 59 l0 44 35 -6 c105 -17 213 93 191 194 -23 105 -1"+
            "48 164 -251 119z m119 -112 c20 -20 20 -43 0 -72 -18 -26 -50 -28 -76 -5 -36 "+
            "33 -13 93 37 93 13 0 31 -7 39 -16z'/></g></svg>";
        let icon1n=n1+b1;
        let icon1h=h1+b1;

        let n2=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23607d8b'>";
        let h2=
            "data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' "+
            "width='26' height='26' viewBox='0 0 64 64' style='fill: %23ffffff'>";
        let b2=
            "<g transform='translate(0,64) scale(0.1,-0.1)'>"+
            "<path d='M267 519 c-48 -28 -61 -116 -23 -154 87 -87 217 6 161 115 -8 16 -24"+
            " 34 -34 40 -25 13 -81 12 -104 -1z'/>"+
            "<path d='M255 291 c-51 -23 -101 -79 -116 -132 -20 -64 -7 -69 181 -69 188 0 "+
            "201 5 181 69 -15 53 -65 109 -116 132 -50 23 -80 23 -130 0z'/></g></svg>";
        let icon2n=n2+b2;
        let icon2h=h2+b2;


        let aambh_style=
            '<style id="aambh_style">'+
            '._eThsBznJ { margin: 1px 0 0 16px !important; } '+
            '._eThsBznJ svg { border: 1px solid #aaa; border-radius: 4px; '+
            'padding: 4px; margin: 0 4px 0 0; vertical-align: -9px; fill: transparent; } '+
            '._eThsBznJ:hover { opacity: 1; } '+
            '._eThsBznJ:hover svg { transition: .2s; background: #00bbff; } '+
            '._eThsBznJ:nth-child(2) svg { background-image: url("'+ icon0n +'"); } '+
            '._eThsBznJ:nth-child(2):hover svg { background-image: url("'+ icon0h +'"); } '+
            '._eThsBznJ:nth-child(3) svg { background-image: url("'+ icon1n +'"); } '+
            '._eThsBznJ:nth-child(3):hover svg { background-image: url("'+ icon1h +'"); } '+
            '._eThsBznJ:nth-child(4) svg { background-image: url("'+ icon2n +'"); } '+
            '._eThsBznJ:nth-child(4):hover svg { background-image: url("'+ icon2h +'"); } '+

            '#commentsHeader h4, h1.commentTitle, #comment_module h3.title '+
            '{ cursor: pointer; } '+ // コメント欄のデザイン
            '</style>';
        if(!document.querySelector('#aambh_style')){
            document.documentElement.insertAdjacentHTML('beforeend', aambh_style); }



        let toHome=document.querySelector('._eThsBznJ');
        toHome.addEventListener('click', function(event){
            event.preventDefault();
            if(event.shiftKey){
                location.href='https://www.ameba.jp/'; }
            else{
                if(document.referrer=='https://www.ameba.jp/'){
                    window.close(); }
                else{
                    window.location.href='https://www.ameba.jp/'; }}});

    } // main()

} // 通常のブログページ等




if(location.href=='https://www.ameba.jp/home'){ //「HOME」画面の場合

    setTimeout(()=>{
        double_reload_ck();
    }, 1000);

    function double_reload_ck(){
        let reload_c=localStorage.getItem('aid_ambh_reload'); // ストレージから取得
        if(reload_c==null){
            reload_c='0';
            localStorage.setItem('aid_ambh_reload', reload_c); } // ストレージに登録
        else if(reload_c=='1'){
            reload_c='0';
            localStorage.setItem('aid_ambh_reload', reload_c); // ストレージに登録
            window.location.reload(false); }}



    let clean_edit=0; // 表示・非表示設定 作業フラグ

    let target1=document.querySelector('head');
    let monitor1=new MutationObserver(main);
    monitor1.observe(target1, { childList: true });

    main();

    function main(){
        let blog_link=document.querySelector('.HomeBlogModule_BlogTitle_Link');
        if(blog_link){
            blog_link.setAttribute('target', '_blank'); }

        let lback=document.querySelector('.HomeBlogModule_LookBackEntry_Article');
        if(lback){
            lback.setAttribute('target', '_blank'); }



        //「ブログ管理」のリンクが無い時に代りのボタンを表示
        setTimeout(()=>{
            Add_acc(); }, 2000);

        function Add_acc(){
            let HBT=document.querySelector('.HomeBlogModule_BlogTitle');
            let HAC=document.querySelector('.HomeBlogModule_Access_Container');
            if(HBT){
                if(!HAC){
                    let hac_h='<a class="HomeBlogModule_Access_Container Tap_Transparent extra" '+
                        'href="https://blog.ameba.jp/ucs/top.do">管理トップ画面を開く</a>'+
                        '<style>.HomeBlogModule_Access_Container.extra { position: relative; '+
                        'height: 3px; padding: 10px 40px 20px; text-align: right; text-decoration: none; } '+
                        '.HomeBlogModule_Access_Container.extra::before, '+
                        '.HomeBlogModule_Access_Container.extra::after { top: 6px !important; }</style>';

                    let extra=document.querySelector('.extra');
                    if(!extra){
                        HBT.insertAdjacentHTML('afterend', hac_h); }}}}



        // 「お知らせ」の表示をシンプルに見易くする
        slim_notify();

        function slim_notify(){
            let note_button=document.querySelectorAll('.HomeRedNotification_Item button');
            for(let k=0; k<note_button.length; k++){
                note_button[k].style.margin='2px 0';
                let n_span=note_button[k].querySelector('span');
                if(clean_edit==0){
                    ellipsis(n_span); }

                if(n_span){
                    if(n_span.textContent.includes('あなたのブログ記事に新しくいいね！')){
                        n_span.innerHTML=short_i(n_span); }
                    else if(n_span.textContent.includes('に承認待ちコメントがあります')){
                        n_span.innerHTML=short_c1(n_span); }
                    else if(n_span.textContent.includes('に承認待ちのコメントがつきました')){
                        n_span.innerHTML=short_c2(n_span); }
                    else if(n_span.textContent.includes('にコメントしました')){
                        n_span.innerHTML=short_c3(n_span); }

                    else if(n_span.textContent.includes('へのコメントが承認されました')){
                        n_span.innerHTML=short_c4(n_span);
                        coment_frame(n_span); }
                    else if(n_span.textContent.includes('に返信しました')){
                        n_span.innerHTML=short_c5(n_span);
                        coment_frame(n_span); }

                    else if(n_span.textContent.includes('メッセージが届きました')){
                        n_span.innerHTML=short_m(n_span); }
                    else if(n_span.textContent.includes('というメッセージが届いています')){
                        n_span.innerHTML=short_m2(n_span); }
                    else if(n_span.textContent.includes('さんがフォロワーになりました')){
                        n_span.innerHTML=short_f(n_span); }
                    else if(n_span.textContent.includes('さんからフォロー申請が届いています')){
                        n_span.innerHTML=short_f1(n_span); }
                    else if(n_span.textContent.includes('さんがフォロー申請を承認しました')){
                        n_span.innerHTML=short_f2(n_span); }
                    else if(n_span.textContent.includes('さんからアメンバー申請が届いています')){
                        n_span.innerHTML=short_a(n_span); }
                    else if(n_span.textContent.includes('さんがアメンバー申請を承認しました')){
                        n_span.innerHTML=short_a1(n_span); }
                    else if(n_span.textContent.includes('マンガクーポンが')){
                        n_span.innerHTML=short_b(n_span); }
                }}


            function short_i(n_p){
                return 'いいね！<b style="color: #aaa">✤</b> ブログにいいね！がつきました'; }

            function short_c1(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('コメントがあります'));
                return 'コメント<b style="color: red">✤</b> '+ np; }

            function short_c2(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('のコメントがつきました'));
                return 'コメント<b style="color: red">✤</b> '+ np; }

            function short_c3(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('コメントしました'));
                return 'コメント<b style="color: red">✤</b> '+ np; }

            function short_c4(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('が承認されました'));
                return 'コメント承認済<b style="color: red">✤</b> '+ np; }

            function short_c5(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('に返信しました'));
                return '返信コメント<b style="color: red">✤</b> '+ np; }

            function short_m(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('メッセージが届きました'));
                return 'メッセージ<b style="color: #ff9800">✤</b> '+ np +'着信'; }

            function short_m2(n_p){
                let np=n_p.textContent;
                let ans=np.match(/「.*?」というメッセージが届いています/)
                if(ans){
                    np=np.replace(ans, '');
                    return 'メッセージ<b style="color: #ff9800">✤</b> '+ np +'着信'; }}

            function short_f(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('がフォロワーになりました'));
                return 'フォロー<b style="color: #4caf50">✤</b> '+ np +'がフォロー'; }

            function short_f1(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('からフォロー申請が届いています'));
                return 'フォロー申請<b style="color: #4caf50">✤</b> '+ np +'が申請'; }

            function short_f2(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('がフォロー申請を承認しました'));
                return 'フォロー承認済<b style="color: #4caf50">✤</b> '+ np +'が承認'; }

            function short_a(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('からアメンバー申請が届いています'));
                return 'アメンバー申請<b style="color: #2196f3">✤</b> '+ np +'が申請'; }

            function short_a1(n_p){
                let np=n_p.textContent;
                np=np.substring(0, np.indexOf('がアメンバー申請を承認しました'));
                return 'アメンバー承認済<b style="color: #2196f3">✤</b> '+ np +'が承認'; }

            function short_b(n_p){
                let np=n_p.textContent;
                np=np.substring(np.indexOf('マンガクーポンが') + 8);
                np=np.substring(0, np.indexOf('あります。'));
                if(np.includes('件')){
                    np=np +'あります'; }
                else{
                    np='1件あります'; }
                return 'マンガクーポン <b style="color: #2196f3">▩</b> '+ np; }


            function ellipsis(n_p){
                n_p.style.cssText=
                    'display: block; width: 340px; padding-left: 5px; '+
                    'overflow: hidden; white-space: nowrap; text-overflow: ellipsis; '; }


            function coment_frame(n_span){
                let button=n_span.closest('button');
                if(button){
                    button.style.boxShadow='-8px 0 0 0 #cfd8dc';
                    button.classList.add('c_f'); }}

        } // slim_notify()



        // 特定の文字列を含む「お知らせ」の選択非表示
        setTimeout(()=>{
            cleaner();
        }, 2000);

        function cleaner(){
            let set_word; // ストレージ登録データ
            let d_word=[]; // 検索語の配列

            clear_disp(0);

            let HUPB_h3=document.querySelector('.HomeUserProfileBody h3');
            if(HUPB_h3){

                let SVG_ambh=
                    '<svg id="svg_ambh" viewBox="0 0 150 150" style="height: 16px; '+
                    'margin: 2px 10px -3px; fill: #2196f3; cursor: pointer;">'+
                    '<path d="M66 13C56 15 47 18 39 24C-12 60 18 146 82 137C92 135 '+
                    '102 131 110 126C162 90 128 4 66 13M68 25C131 17 145 117 81 125C16 '+
                    '133 3 34 68 25M69 40C61 41 39 58 58 61C66 63 73 47 82 57C84 60 '+
                    '83 62 81 65C77 70 52 90 76 89C82 89 82 84 86 81C92 76 98 74 100 66'+
                    'C105 48 84 37 69 40M70 94C58 99 66 118 78 112C90 107 82 89 70 94z">'+
                    '</path></svg>'+
                    '<style>'+
                    '.HomeUserProfileBody h3 { cursor: pointer; } '+
                    '.HomeUserProfileBody h3:hover { color: #fff; '+
                    'box-shadow: inset 0 21px 0 0 #237988, 0 -2px 0 2px #237988; }</style>';

                if(!document.querySelector('#svg_ambh')){
                    HUPB_h3.insertAdjacentHTML('afterend', SVG_ambh); }

                let help=document.querySelector('#svg_ambh');
                if(help){
                    help.onclick=function(){
                        let url='https://ameblo.jp/personwritep/entry-12736445680.html';
                        window.open(url, '_blank'); }}


                HUPB_h3.onclick=function(){
                    clean_edit=1;

                    let clean_box=
                        '<div id="clean_box">'+
                        '<input id="word_set" type="text"> '+
                        '<input id="clean_set" type="button" value="Set">　'+
                        '<input id="close" type="button" value="✖">'+
                        '</div>'+
                        '<style>'+
                        '#clean_box { position: fixed; top: 70px; left: calc(50% - 540px); '+
                        'font: normal 16px Meiryo; padding: 20px; background: #fff; '+
                        'border: 1px solid #aaa; box-shadow: 10px 20px 50px rgb(0 0 0 / 25%); } '+
                        '@media screen and (max-width: 1140px){ #clean_box { left: 20px; }} '+
                        '#word_set { width: 300px; padding: 4px 6px 2px; } '+
                        '#clean_set, #close { padding: 4px 4px 2px; }'+
                        '</style>';

                    if(!document.querySelector('#clean_box')){
                        document.body.insertAdjacentHTML('beforeend', clean_box); }
                    else{
                        clean_edit=0;
                        clear_disp(0);
                        document.querySelector('#clean_box').remove(); }

                    let word_set=document.querySelector('#clean_box #word_set');
                    let clean_set=document.querySelector('#clean_box #clean_set');
                    let close=document.querySelector('#clean_box #close');
                    if(word_set){

                        clear_disp(1);

                        word_set.value=set_word; // 初期値を表示
                        word_set.addEventListener('change', function(){
                            let input_word=word_set.value;
                            clean_set.onclick=function(){
                                let tmp_word=input_word.split(/[\x20\u3000]/);
                                tmp_word.forEach((val)=>{
                                    val=val.replace(/\s/g, ''); }) // 配列全要素から再度 空白文字を削除
                                tmp_word=tmp_word.filter(Boolean); // 空要素を削除
                                word_set.value=tmp_word.join(' ');
                                set_word=word_set.value;
                                let write_json=JSON.stringify(set_word);
                                localStorage.setItem('aid_ambh_set', write_json); // ローカルストレージ保存

                                clear_disp(1); }});

                        close.onclick=function(){
                            clean_edit=0;
                            clear_disp(0);
                            document.querySelector('#clean_box').remove(); }

                    }}} // if(HUPB_h3)


            function clear_disp(n){
                let HUPB_h3=document.querySelector('.HomeUserProfileBody h3');

                let read_json=localStorage.getItem('aid_ambh_set'); // ローカルストレージ保存名
                set_word=JSON.parse(read_json);
                if(set_word==null){
                    set_word=''; }

                d_word=set_word.split(/\x20/);
                let reg_text=d_word.join('|');
                let regexp=new RegExp(reg_text);

                let count=0;
                let HRN_I=document.querySelectorAll('.HomeRedNotification_Item');
                for(let k=0; k<HRN_I.length; k++){
                    let text=HRN_I[k].textContent;
                    let button=HRN_I[k].querySelector('button');
                    let span=HRN_I[k].querySelector('span');
                    if(set_word!='' && regexp.test(text)){ // 設定した語がヒットする場合
                        count+=1;
                        if(n==0 && clean_edit==0){
                            HRN_I[k].style.display='none';
                            button.style.pointerEvents='auto';
                            button.style.userSelect='none';
                            span.style.whiteSpace='nowrap'; }
                        if(n==1){
                            HRN_I[k].style.display='list-item';
                            HRN_I[k].style.background='#e4ecf1';
                            button.style.pointerEvents='none';
                            button.style.userSelect='auto';
                            span.style.whiteSpace=''; }}
                    else{
                        if(n==0 && clean_edit==0){
                            HRN_I[k].style.display='list-item';
                            button.style.pointerEvents='auto';
                            button.style.userSelect='none';
                            span.style.whiteSpace='nowrap'; }
                        if(n==1){
                            HRN_I[k].style.display='list-item';
                            HRN_I[k].style.background='';
                            button.style.pointerEvents='none';
                            button.style.userSelect='auto';
                            span.style.whiteSpace=''; }}}

                let titlebadge=document.querySelector('.PcToggleModule_TitleBadge');
                if(titlebadge){
                    HRN_I.length - count
                    titlebadge.textContent=HRN_I.length - count;
                    if(count>0){
                        if(HUPB_h3){
                            let span_c=
                                '<span class="db" style="margin-left: 8px; padding: 0 6px; '+
                                'font-size: 1.4rem; color: #fff; background: #cfe1eb; '+
                                'border-radius: 30px; vertical-align: 2px;">'+ count +'</span>';

                            if(!HUPB_h3.querySelector('.db')){
                                HUPB_h3.insertAdjacentHTML('beforeend', span_c); }}}
                    else{
                        if(HUPB_h3.querySelector('.db')){
                            HUPB_h3.querySelector('.db').remove(); }}}

            } // clear_disp(n)
        } // cleaner()



        //「全てのお知らせを見る」から「お知らせ」画面をフレーム表示する
        setTimeout(()=>{
            notify_frame();
        }, 400);

        function notify_frame(){

            if_creat(); // 常にフレームを配置


            let HNI_link=document.querySelector('.HomeRedNotification_Item_Other_Link');
            let if_n=document.querySelector('#notify');
            if(HNI_link && if_n){
                HNI_link.removeAttribute('href');

                HNI_link.onclick=function(event){
                    if(!event.shiftKey){
                        if_n.classList.toggle('open'); } //「すべてのお知らせを見る」でフレーム開閉
                    else{
                        location.href="/notifications"; }}}


            let note_button_com=document.querySelectorAll('.HomeRedNotification_Item button.c_f');
            for(let k=0; k<note_button_com.length; k++){
                note_button_com[k].onclick=function(event){
                    event.preventDefault();
                    event.stopImmediatePropagation();
                    let bt=note_button_com[k].textContent;
                    bt=bt.substring(bt.indexOf('✤') +2);
                    let link_url=com_url(bt);
                    if(link_url){
                        window.open(link_url, '_blank');
                        double_reload();
                    }}} //「承認・返信通知」でコメント欄へ直行


            function com_url(bt){
                let bt_url;
                let if_n=document.querySelector('#notify');
                if(if_n){
                    let if_body=if_n.contentWindow.document.body;
                    if(if_body){
                        let NLIM=if_body.querySelectorAll('.NotificationListItem_Message');
                        for(let k=0; k<NLIM.length; k++){
                            let notify_str=decodeEntity(NLIM[k].textContent);
                            if(notify_str.includes(bt)){
                                let link_elem=NLIM[k].closest('.NotificationListItem');
                                if(link_elem){
                                    bt_url=link_elem.getAttribute('href');
                                    break; }}}
                        return bt_url; }}


                function decodeEntity(str){
                    var textarea=document.createElement('textarea');
                    textarea.innerHTML=str;
                    return textarea.value; }

            } // com_url(bt)


            function double_reload(){
                let reload_c=localStorage.getItem('aid_ambh_reload'); // ストレージから取得
                if(reload_c==null || reload_c=='0'){
                    reload_c='1';
                    localStorage.setItem('aid_ambh_reload', reload_c); // ストレージに登録
                    window.location.reload(false); }}


            function if_creat(){
                let if_elem=
                    '<iframe id="notify" scrolling="no" src="https://www.ameba.jp/notifications">'+
                    '</iframe>'+
                    '<style>'+
                    '#notify { position: relative; width: 664px; height: 0; margin-bottom: 0; '+
                    'border: none; background: #fff; } '+
                    '#notify.open { animation: Open .4s forwards; } '+
                    '@keyframes Open { '+
                    '0% { height: 0; margin-bottom: 0; } '+
                    '100% { height: 400px; margin-bottom: 10px; }} '+
                    '</style>';

                let PLRC=document.querySelector('.PcLayout_RightColumn');
                if(!document.querySelector('#notify') && PLRC){
                    PLRC.insertAdjacentHTML('afterbegin', if_elem); }


                let if_n=document.querySelector('#notify');
                if_n.onload=function(){
                    let retry1=0;
                    let interval1=setInterval(wait_target1, 10);
                    function wait_target1(){
                        retry1++;
                        if(retry1>100){ // リトライ制限 1secまで
                            clearInterval(interval1); }
                        let if_body=if_n.contentWindow.document.body;
                        if(if_body){
                            clearInterval(interval1);
                            set_iframe(if_body);
                            setTimeout(()=>{
                                add_hash_i(if_body);
                            }, 100); }}
                } //  if_n.onload


                function set_iframe(if_body){
                    let in_style=
                        '<style class="in_style">'+
                        'body { background: #fff !important; } '+
                        '.PcHeader, .PageNotifications_Headline { display: none; } '+
                        '.PageNotifications { width: 657px; height: 380px; overflow-y: scroll; '+
                        'margin: 10px 12px !important; padding: 0 15px 0 0 !important; }'+
                        '</style>';

                    if_body.insertAdjacentHTML('beforeend', in_style);

                } // set_iframe()


                function add_hash_i(if_body){
                    let set_word; // ストレージ登録データ: 「お知らせ」の非表示語の登録
                    let d_word=[]; // 検索語の配列
                    let read_json=localStorage.getItem('aid_ambh_set'); // ローカルストレージ保存名
                    set_word=JSON.parse(read_json);
                    if(set_word==null){
                        set_word=''; }
                    d_word=set_word.split(/\x20/);
                    let reg_text=d_word.join('|');
                    let regexp=new RegExp(reg_text);


                    let notifi_link=if_body.querySelectorAll('.NotificationListItem');
                    for(let k=0; k<notifi_link.length; k++){
                        let href=notifi_link[k].getAttribute('href');
                        notifi_link[k].setAttribute('target', '_blank'); // 全て別タブで開く設定に変更

                        if(href.match(/comment-/) && !href.match(/#cbox/)){ //「コメント承認・返信」
                            notifi_link[k].setAttribute('href', href+'#cbox');
                            notifi_link[k].style.boxShadow=
                                'inset -2px 0 0 2px #fff, inset 8px 0 0 #cfd8dc'; }

                        else if(href.match(/msg\.ameba/)){ //「メッセージ」のお知らせ
                            let text=notifi_link[k].textContent;
                            if(set_word!='' && regexp.test(text)){ // 設定した語がヒットする場合
                                notifi_link[k].style.display='none'; }}}

                } //  add_hash_i()

            } // if_creat()
        } // notif_frame()

    } // main()




    //「フォローフィード」のリンクを「Shift+Click」で直接「コメント欄を開く」
    let target2=document.querySelector('.HomeChecklist');
    let monitor2=new MutationObserver(add_hash_f);
    monitor2.observe(target2, {childList: true, subtree: true});

    add_hash_f();

    function add_hash_f(){
        let HCCI=document.querySelectorAll('.HomeChecklist_Collection_Item');
        for(let k=0; k<HCCI.length; k++){
            let HCA_link=HCCI[k].querySelector('a.HomeChecklist_Article_Link');
            if(HCA_link){
                HCA_link.onclick=function(event){
                    if(event.shiftKey){
                        event.preventDefault();
                        let href=HCA_link.getAttribute('href');

                        let M_icon=HCCI[k].querySelector('.HomeChecklist_Article_Meta_Icon');
                        if(M_icon){
                            window.open(href+'#cbox', '_blank');
                            setTimeout(()=>{
                                all_item_bar(0);
                                all_item_bar_s(0);
                                all_item_bar_c(0);
                            }, 400); }
                        else{
                            window.open(href, '_blank'); }}}}}

        document.addEventListener('keydown', function(event){
            if(event.shiftKey){
                all_item_bar(1); }});

        document.addEventListener('keyup', function(event){
            if(!event.shiftKey){
                all_item_bar(0); }});

    } // add_hash_f()



    //「コメントした記事」のリンクを直接「コメント欄を開く」仕様に変更
    let retry0=0;
    let interval0=setInterval(wait_target0, 400);
    function wait_target0(){
        retry0++;
        if(retry0>20){ // 8sec まで観測
            clearInterval(interval0); }
        let target=document.querySelector('.HomeBlogComment_Collection a');
        if(target){
            clearInterval(interval0);
            add_hash_c();
            later(); }}


    function later(){
        let more_c=document.querySelector('.HomeBlogComment .Collection_ReadMore_Button');
        if(more_c){
            more_c.onclick=function(){
                setTimeout(()=>{
                    add_hash_c(); }, 1000); }}} // later()


    function add_hash_c(){
        let HBCC_link=document.querySelectorAll('.HomeBlogComment_Collection a');
        for(let k=0; k<HBCC_link.length; k++){
            HBCC_link[k].onclick=function(event){
                if(event.shiftKey){
                    event.preventDefault();
                    let href=HBCC_link[k].getAttribute('href');
                    window.open(href+'#cbox', '_blank');
                    setTimeout(()=>{
                        all_item_bar(0);
                        all_item_bar_s(0);
                        all_item_bar_c(0);
                    }, 400); }}}

        document.addEventListener('keydown', function(event){
            if(event.shiftKey){
                all_item_bar_c(1); }});

        document.addEventListener('keyup', function(event){
            if(!event.shiftKey){
                all_item_bar_c(0); }});



        //「スタッフブログ」のリンクを「Shift+Click」で直接「コメント欄を開く」
        let stuff_link=document.querySelector('.PcModuleNotification_Link');
        if(stuff_link){
            stuff_link.onclick=function(event){
                if(event.shiftKey){
                    event.preventDefault();
                    let href=stuff_link.getAttribute('href');
                    window.open(href+'#cbox', '_blank');
                    setTimeout(()=>{
                        all_item_bar(0);
                        all_item_bar_s(0);
                        all_item_bar_c(0);
                    }, 400); }}}

        document.addEventListener('keydown', function(event){
            if(event.shiftKey){
                all_item_bar_s(1); }});

        document.addEventListener('keyup', function(event){
            if(!event.shiftKey){
                all_item_bar_s(0); }});

    } // add_hash_c()



    function all_item_bar(n){
        let HCCI=document.querySelectorAll('.HomeChecklist_Collection_Item');
        for(let k=0; k<HCCI.length; k++){
            if(n==0){
                HCCI[k].style.boxShadow=''; }
            else{
                let M_icon=HCCI[k].querySelector('.HomeChecklist_Article_Meta_Icon');
                if(M_icon){
                    HCCI[k].style.boxShadow='-10px 0 0 -2px #cfd8dc'; }}}}

    function all_item_bar_s(n){
        let stuff_link=document.querySelector('.PcModuleNotification_Link');
        if(stuff_link){
            if(n==0){
                stuff_link.style.boxShadow=''; }
            else{
                stuff_link.style.boxShadow='-8px 0 0 #cfd8dc'; }}}


    function all_item_bar_c(n){
        let HBCC_link=document.querySelectorAll('.HomeBlogComment_Collection a');
        for(let k=0; k<HBCC_link.length; k++){
            if(n==0){
                HBCC_link[k].style.boxShadow=''; }
            else{
                HBCC_link[k].style.boxShadow='-10px 0 0 -2px #cfd8dc'; }}}

} //「HOME」画面の場合




if(location.href=='https://www.ameba.jp/notifications'){ //「お知らせ」画面の場合
    let pc_logo=document.querySelector('h1.PcHeader_Logo');
    if(pc_logo){
        pc_logo.title="◀ HOME";
        pc_logo.onclick=function(e){
            e.preventDefault();
            window.location.href='https://www.ameba.jp/home'; }}


    let target3=document.querySelector('head');
    let monitor3=new MutationObserver(add_hash_n);
    monitor3.observe(target3, { childList: true });

    add_hash_n();

    function add_hash_n(){
        let notifi_link=document.querySelectorAll('.NotificationListItem');
        for(let k=0; k<notifi_link.length; k++){
            let href=notifi_link[k].getAttribute('href');
            if(href.match(/comment-/) && !href.match(/#cbox/)){ //「コメント承認」
                notifi_link[k].setAttribute('href', href+'#cbox');
                notifi_link[k].setAttribute('target', '_blank');
                notifi_link[k].style.boxShadow='inset -2px 0 0 2px #fff, inset 8px 0 0 #cfd8dc'; }}

        let mannga_count=0;
        for(let k=0; k<notifi_link.length; k++){
            let href=notifi_link[k].getAttribute('href');
            if(href.match(/dokusho-ojikan.jp/)){ //「マンガクーポン」
                mannga_count+=1;
                if(mannga_count>1){
                    notifi_link[k].remove(); }}}

        let unei_count=0;
        for(let k=0; k<notifi_link.length; k++){
            let message=notifi_link[k].querySelector('.NotificationListItem_Message');
            if(message.textContent.includes('アメーバ運営局からメッセージが')){ //「運営メッセージ」
                unei_count+=1;
                if(unei_count>1){
                    notifi_link[k].remove(); }}}
    } //  add_hash_n()
} //「お知らせ」画面の場合




if(location.pathname.startsWith('/ucs/comment/commentlist')){ //「コメント管理」画面の場合
    let UL_link=document.querySelectorAll('.userList__titleLink');
    for(let k=0; k<UL_link.length; k++){
        UL_link[k].onclick=function(event){
            if(event.shiftKey){
                event.preventDefault();
                let href=UL_link[k].getAttribute('href');
                window.open(href+'#cbox', null, '_blank');
                setTimeout(()=>{
                    all_item_rest();
                }, 1000); }}}


    document.onkeydown=function(event){
        if(event.shiftKey){
            let ULI=document.querySelectorAll('.userList__item');
            for(let k=0; k<ULI.length; k++){
                ULI[k].style.boxShadow='-1px 0 #fff, -8px 0 #cfd8dc'; }}}


    document.onkeyup=function(event){
        if(!event.shiftKey){
            all_item_rest(); }}


    function all_item_rest(){
        let ULI=document.querySelectorAll('.userList__item');
        for(let k=0; k<ULI.length; k++){
            ULI[k].style.boxShadow=''; }}
} //「コメント管理」画面の場合




if(location.pathname=='/ucs/iine/list.html'){ //「いいね！履歴」ページで有効
    if(document.referrer=='https://www.ameba.jp/'){
        window.location.href='https://blog.ameba.jp/ucs/top.do'; }}




if(location.hostname=='msg.ameba.jp'){ //「メッセージ」ページで有効
    let profile=document.querySelector('.ucsSubenu-item a[title="プロフィール"]');
    if(profile){
        profile.textContent='管理トップ';
        profile.title='管理トップ';
        profile.target='_self';
        profile.href='https://blog.ameba.jp/ucs/top.do';
        let css=
            '<style>'+
            '#ucsSubMenu li:nth-child(2) a:before { content: "\\EA31" !important; '+
            'font-size: 23px !important; position: absolute; }</style>'
        profile.insertAdjacentHTML('afterend', css); }}
