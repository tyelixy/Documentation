<!-- docs/_sidebar.md -->

* [入门指南](rumen)
* [书籍](books/books) 
   * [图解HTTP](books/http)
* [破解软件](pojie/jieshao)
   * [burpsuit](pojie/burpsuit)
   * [vmware](pojie/vmware)
* [内核]()
* [堆]()  
* [资料]()  
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
.sidebar-nav-parent-li {
      position: relative;
    }

    .sidebar-nav {
      padding-right: 10px;
    }

    .sidebar-nav .ul-after {
      content: '';
      width: 10px;
      height: 30px;
      /* background: red; */
      position: absolute;
      right: 10px;
      top: 0px;
      display: flex;
      align-items: center;
    }

    .ul-after-sanjiao {
      width: 0;
      height: 0;
      border-left: 8px solid #cecece;
      border-top: 8px solid transparent;
      border-bottom: 8px solid transparent;
      transition: transform 0.3s;
      position: relative;

    }

    .ul-after-sanjiao::after {
      content: '';
      width: 0;
      height: 0;
      position: absolute;
      top: -8px;
      left: -11px;
      border-left: 8px solid #fff;
      border-top: 8px solid transparent;
      border-bottom: 8px solid transparent;
    }

    .ul-after-sanjiao:hover {
      border-left: 8px solid #42b983;
    }

    .sidebar-nav-parent-li-shouqi .ul-after-sanjiao {
      transform: rotate(90deg);
      transition: transform 0.3s;
    }
plugins: [
        function (hook) {
          hook.doneEach(function () {

            let max_cengji = 3;    //默认展开层级深度
            //添加箭头
            $('.sidebar-nav').find('li').each(function (i, e) {
              var el = e;
              if (el.nextSibling && el.nextSibling.tagName == 'UL') {
                $(el).addClass('sidebar-nav-parent-li').append(
                  "<div class='ul-after'><div class='ul-after-sanjiao'></div></div>");
              }
            })

            //初始时候展开层级

            function find_children_li(dom, current_cengji) {
              if (dom) {
                current_cengji++;
                $($(dom).find('li').each(function (i, e) {
                  var el = e;
                  if (el.nextSibling && el.nextSibling.tagName == 'UL') {
                    if (current_cengji >= max_cengji) {
                      $(el.nextSibling).hide();
                      $(el).addClass('sidebar-nav-parent-li-shouqi');
                    }
                    find_children_li(el.nextSibling, current_cengji);
                  }
                }))
              } else {
                return
              }
            }

            find_children_li($('.sidebar-nav')[0], 0);

            //绑定事件
            $('.ul-after-sanjiao').on('click', function (e) {
              e.stopPropagation();
              e.preventDefault();
              var el = e.target.parentNode.parentNode.nextSibling;
              if (el) {
                $(el).toggle();
                if ($(e.target.parentNode.parentNode).hasClass('sidebar-nav-parent-li-shouqi')) {
                  $(e.target.parentNode.parentNode).removeClass('sidebar-nav-parent-li-shouqi');
                } else {
                  $(e.target.parentNode.parentNode).addClass('sidebar-nav-parent-li-shouqi');
                }
              }
            })
          })
        }
      ]
