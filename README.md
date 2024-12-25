# pakamsuch.github.io
<?session_start();?>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>K-borad 입니다.</title>
    <link rel="stylesheet" type="text/css" href="mystyle.css"/>
    <style>
    *{
      background-color: white;

    }
    body{
    }
    header {
      width: 100%;
      height: 100px;
    }
    nav {
      width: 100%;
      height: 23px;
      border-top: 1px solid red;
      border-bottom: 1px solid red;
      margin-right: 10%;
      background-color: black;
      color: white;
    }
    nav a{
      background-color: black;
      color: white;
      text-decoration: none;
    }

      #left{
        position: fixed;
        left: 30px;top: 200px;bottom: 0;
        width: 200px;
      }
      #left ul{
        background-color: black;
        list-style: none;
        margin: 0;
        padding: 0;
      }
      #left ul li{
        margin-left: 20px;
        padding-top: 5px;
        background-color: black;
      }
      #left ul li a{
        background-color: black;
        color: white;
        text-decoration: none;
      }
      #main{
        padding-left: 250px;
        padding-top: 20px;
        left: 250px;top: 200px;bottom: 0;
        width: 60%;
        height: 500px;
      }
    footer{
      background-image: url('images/footer_bg.gif');
      width: 100%;
      position: fixed;
      left: 0;top: 700px;bottom: 0;
      height: 50px;
      clear: both;
      text-align: center;
    }
      .f{
        text-align: center;
      }
    </style>
  </head>
  <body>
    <header align=center>
      <h1>
        <a href="index.php">K-board</a>
      </h1>
    </header>
    <?
      if($_SESSION['userid']){

    echo "<nav align=right>
      안녕하세요 ".$_SESSION['user_nic']."님&nbsp&nbsp&nbsp
      <a href='logout.php'>로그아웃</a>
      <a target='iframe1' href='my_page.php'>마이페이지</a>
    </nav>";

    }
    else{
    ?>
    <nav align=right>
      <a href="login.php">로그인</a>
      <a href="sign_up.php">회원가입</a>
    </nav>
    <?
    }
    ?>
    <aside id="left">
      <h4>카테고리</h4>
      <ul>
        <li><a target="iframe1" href="board.php?board_id=notice">공지사항</a></li>
        <li><a target="iframe1" href="board.php?board_id=board">자유게시판</a></li>

        <li><a target="iframe1" href="board.php?board_id=music">음악</a></li>
        <li><a target="iframe1" href="board.php?board_id=movie">영화</a></li>
      </ul>
    </aside>
    <section id="main">
      <article id="article1">
        <iframe name="iframe1" src="main.php" width="1000px" height="700px" seamless></iframe>
      </article>
    </section>
    <footer>Copyright (c) 2020 k_board</footer>
  </body>
</html>
<?php
  include ('db_connect.php');
  $board_id = $_GET['board_id'];
?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="css/style.css" />
    <title></title>
  </head>
  <body>
    <div id="board_area">

      <h1>공지사항</h1>
      <table class="list-table">
      <thead>
          <tr>
              <th width="70">번호</th>
                <th width="500">제목</th>
                <th width="120">글쓴이</th>
                <th width="100">작성일</th>
                <th width="100">조회수</th>
            </tr>
        </thead>
        <?php
        // board테이블에서 idx를 기준으로 내림차순해서 5개까지 표시
          $sql = mq("select * from notice order by idx desc limit 0,2");
            while($board = $sql->fetch_array())
            {
              //title변수에 DB에서 가져온 title을 선택
              $title=$board["title"];
              if(strlen($title)>30)
              {
                //title이 30을 넘어서면 ...표시
                $title=str_replace($board["title"],mb_substr($board["title"],0,30,"utf-8")."...",$board["title"]);
              }
        ?>
      <tbody>
        <tr>
          <td width="70"><?php echo $board['idx']; ?></td>
          <td width="500"><a href="read.php?board_id=notice&idx=<?php echo $board["idx"];?>"><?php echo $title;?></a></td>
          <td width="120"><?php echo $board['name']?></td>
          <td width="100"><?php echo $board['date']?></td>
          <td width="100"><?php echo $board['hit']; ?></td>
        </tr>
      </tbody>
      <?php } ?>
    </table>
    <br><br><br>
      <h1>자유게시판</h1>
      <table class="list-table">
      <thead>
          <tr>
              <th width="70">번호</th>
                <th width="500">제목</th>
                <th width="120">글쓴이</th>
                <th width="100">작성일</th>
                <th width="100">조회수</th>
            </tr>
        </thead>
        <?php
        // board테이블에서 idx를 기준으로 내림차순해서 5개까지 표시
          $sql = mq("select * from board order by hit desc limit 0,3");
            while($board = $sql->fetch_array())
            {
              //title변수에 DB에서 가져온 title을 선택
              $title=$board["title"];
              if(strlen($title)>30)
              {
                //title이 30을 넘어서면 ...표시
                $title=str_replace($board["title"],mb_substr($board["title"],0,30,"utf-8")."...",$board["title"]);
              }
        ?>
      <tbody>
        <tr>
          <td width="70"><?php echo $board['idx']; ?></td>
          <td width="500"><a href="read.php?board_id=board&idx=<?php echo $board["idx"];?>"><?php echo $title;?></a></td>
          <td width="120"><?php echo $board['name']?></td>
          <td width="100"><?php echo $board['date']?></td>
          <td width="100"><?php echo $board['hit']; ?></td>
        </tr>
      </tbody>
      <?php } ?>
    </table>






  </body>
</html>
<?php
	include ('db_connect.php');
  $id = $_SESSION['userid'];
?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
    <style>
    .my_G {
      margin-top: 40px;
    }
    .my_G thead th{
      height:40px;
      border-top:2px solid #09C;
      border-bottom:1px solid #CCC;
      font-weight: bold;
      font-size: 17px;
    }
    .my_G tbody td{
      text-align:center;
      padding:10px 0;
      border-bottom:1px solid #CCC; height:20px;
      font-size: 14px
    }
    .list-table {
      margin-top: 40px;
    }
    .list-table thead th{
      height:40px;
      border-top:2px solid #09C;
      border-bottom:1px solid #CCC;
      font-weight: bold;
      font-size: 17px;
    }
    .list-table tbody td{
      text-align:center;
      padding:10px 0;
      border-bottom:1px solid #CCC; height:20px;
      font-size: 14px
    }
    </style>
  </head>
  <body>
    <h1>내 정보</h1>
    <table class="my_G">
      <?
      $sql = mq("select * from user where id='$id'");
      while($user = $sql->fetch_array()){
      ?>
      <tr>
        <td width="50">닉네임</td>
        <td width="100"><?echo $user["nic_name"];?></td>
      </tr>
      <tr>
        <td>가입일</td>
        <td><?echo $user["user_date"];?></td>
      </tr>
      <?}?>
    </table>
    <h1>내가 쓴글</h1>
    <table class="list-table">
    <thead>
        <tr>
            <th width="70">번호</th>
              <th width="500">제목</th>
              <th width="100">작성일</th>
              <th width="100">조회수</th>
          </tr>
      </thead>
      <?php
      // board테이블에서 idx를 기준으로 내림차순해서 5개까지 표시
        $sql = mq("select * from board where id='$id' order by idx desc" );
          while($board = $sql->fetch_array())
          {
            //title변수에 DB에서 가져온 title을 선택
            $title=$board["title"];
            if(strlen($title)>30)
            {
              //title이 30을 넘어서면 ...표시
              $title=str_replace($board["title"],mb_substr($board["title"],0,30,"utf-8")."...",$board["title"]);
            }
      ?>
    <tbody>
      <tr>
        <td width="70"><?php echo $board['idx']; ?></td>
        <td width="500"><a href="read.php?board_id=board&idx=<?php echo $board["idx"];?>"><?php echo $title;?></a></td>
        <td width="100"><?php echo $board['date']?></td>
        <td width="100"><?php echo $board['hit']; ?></td>
      </tr>
    </tbody>
    <?php } ?>
  </table>
  </body>
</html>
<?php
	include ('db_connect.php');
?>
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>게시판</title>
  <style>
  #board_read {
    width:900px;
    position: relative;
    word-break:break-all;
  }
  #user_info {
    font-size:14px;
  }
  #user_info ul li {
    float:left;
    margin-left:10px;
  }
  #bo_line {
    width:880px;
    height:2px;
    background: gray;
    margin-top:20px;
  }
  #bo_content {
    margin-top:20px;
  }
  #bo_ser {
    font-size:14px;
    color:#333;
    position: absolute;
    right: 0;
  }
  #bo_ser > ul > li {
    list-style: none;
    float:left;
    margin-left:10px;
  }
  /* 댓글 */
.reply_view {
	width:900px;
	margin-top:100px;
	word-break:break-all;
}
.dap_lo {
	font-size: 14px;
	padding:10px 0 15px 0;
	border-bottom: solid 1px gray;
}
.dap_to {
	margin-top:5px;
}
.rep_me {
	font-size:12px;
}
.rep_me ul li {
	float:left;
	width: 30px;
}
.dat_delete {
	display: none;
}
.dat_edit {
	display:none;
}
.dap_sm {
	position: absolute;
	top: 10px;
}
.dap_edit_t{
	width:520px;
	height:70px;
	position: absolute;
	top: 40px;
}
.re_mo_bt {
	position: absolute;
	top:40px;
	right: 5px;
	width: 90px;
	height: 72px;
}
#re_content {
	width:700px;
	height: 56px;
  resize: none;
}
.dap_ins {
	margin-top:50px;
}
.re_bt {
	position: absolute;
	width:100px;
	height:56px;
	font-size:16px;
	margin-left: 10px;
}
#foot_box {
	height: 50px;
}
  </style>
</head>
<body>
	<?php
    $board_id = $_GET['board_id'];
		$bno = $_GET['idx']; /* bno함수에 idx값을 받아와 넣음*/
		$hit = mysqli_fetch_array(mq("select * from ".$board_id." where idx ='".$bno."'"));
		$hit = $hit['hit'] + 1;
		$fet = mq("update ".$board_id." set hit = '".$hit."' where idx = '".$bno."'");
		$sql = mq("select * from ".$board_id." where idx='".$bno."'"); /* 받아온 idx값을 선택 */
		$board = $sql->fetch_array();
	?>
<!-- 글 불러오기 -->
  <div id="board_read">
	   <h2><?php echo $board['title']; ?></h2>
		   <div id="user_info" align=right>
			      <?php echo $board['name']; ?> <?php echo $board['date']; ?> 조회:<?php echo $board['hit']; ?>
				    <div id="bo_line"></div>
			</div>
			<div>
				파일 : <a href="uploads/<?php echo $board['file'];?>" download><?php echo $board['file']; ?></a>
			</div>
			<div id="bo_content">
				<?php echo nl2br("$board[content]"); ?>
			</div>

	<!-- 목록, 수정, 삭제 -->

	   <div id="bo_ser">
		     <ul>
			        <li><a href="board.php?board_id=<?echo $board_id;?>">[목록으로]</a></li>
              <?if ($board['name']==$_SESSION['userid']){?>
			        <li><a href="modify.php?idx=<?php echo $board['idx']; ?>">[수정]</a></li>
			        <li><a href="delete.php?idx=<?php echo $board['idx']; ?>">[삭제]</a></li>
              <?}?>
		    </ul>
	  </div>

  </div>
  <!--- 댓글 불러오기 -->
<div class="reply_view">
	<h3>댓글목록</h3>
		<?php
			$sql3 = mq("select * from ".$board_id."_reply where con_num='".$bno."' order by idx desc");
			while($reply = $sql3->fetch_array()){
		?>
		<div class="dap_lo">
			<div><b><?php echo $reply['name'];?></b></div>
			<div class="dap_to comt_edit"><?php echo nl2br("$reply[content]"); ?></div>
			<div class="rep_me dap_to"><?php echo $reply['date']; ?></div>

		</div>
	<?php } ?>

	<!--- 댓글 입력 폼 -->
	<div class="dap_ins">
		<form action="reply_ok.php?board_id=<?echo $board_id;?>&idx=<?php echo $bno; ?>" method="post">
			<input type="hidden" name="dat_user" id="dat_user" class="dat_user" size="15" placeholder="아이디" value=<?$_SESSION['userid']?>>
			<div style="margin-top:10px; ">
				<textarea name="content" class="reply_content" id="re_content" ></textarea>
				<button id="rep_bt" class="re_bt">댓글</button>
			</div>
		</form>
	</div>
</div><!--- 댓글 불러오기 끝 -->
<div id="foot_box"></div>
</div>
</body>
</html>
<?
  session_start();

  unset($_SESSION['userid']);
  echo "
  <script>
    location.href='index.php'
    </script>";
?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>K-borad 로그인</title>
    <style>
      * {margin: 0; padding: 0;}
      #login_box{
        width:400px;
        height:150px;
        border:solid 2px gray;
        position: absolute;
        left: 50%; top: 50%;
        margin-left: -200px;
        margin-top: -100px;
        background-color: orange;
      }
      .submit{
        width: 80px;
        height: 70px;
        border-radius: 5px;
        position: absolute;
        left: 50%; top: 50%;
        margin-left: 80px;
        margin-top: -60px;
      }
      a{
        color: black;
        text-decoration: none;
      }
    </style>
  </head>
  <body>
    <div id="login_box">
      <form name="loginForm" action="login_search.php" method="post">
        <table width="300" height="100" border="0">
          <tr>
            <th align="right">아 이 디 :</th>
            <td><input type="text" name="userid"></td>
          </tr>
          <tr>
            <th align="right">패스워드 :</th>
            <td><input type="password" name="userpw"></td>
          </tr>
        </table>
        <input type="submit" class="submit" value="로그인">
        <p align=center>
        <a href="sign_up.php">회원가입</a> |
        <a href="#">비밀번호 찾기</a>
        </p>
      </form>

    </div>
  </body>
</html>
<?

  $connect = mysql_connect("localhost","kdhong","1234");
  mysql_set_charset("utf8",$connect);
  mysql_select_db("kdhong_db",$connect);

  $id = $_POST['userid'];
  $pw = $_POST['userpw'];

  if (!$id) {
    echo "
    <script>
      window.alert('아이디를 입력하세요');
      history.go(-1);
      </script>";
  }
  elseif (!$pw) {
    echo "
    <script>
      window.alert('패스워드를 입력하세요')
      history.go(-1)
      </script>";

  }
  else {


  $sql = "select * from user where id='$id'";
  $result = mysql_query($sql,$connect);
  $num1 = mysql_num_rows($result);

  $sql = "select * from user where id='$id' and pw='$pw'";
  $result = mysql_query($sql,$connect);
  $num2 = mysql_num_rows($result);
  if (!$num1) {
    echo "
    <script>
      window.alert('아이디/비밀번호가 틀렸습니다 다시 입력하세요')
      history.go(-1)
      </script>";
  }
  elseif (!$num2) {
    echo "
    <script>
      window.alert('아이디/비밀번호가 틀렸습니다 다시 입력하세요')
      history.go(-1)
      </script>";
  }
  else {
    session_start();
    $user = mysql_fetch_array($result);
    $_SESSION['userid'] = $id;
    $_SESSION['user_nic'] = $user[2];
    echo "
    <script>
      location.href='index.php'
    </script>";
  }
}
mysql_close($connect);
?>

<meta charset="utf-8">
<?php
	session_start();
	header('Content-Type: text/html; charset=utf-8'); 

	$db = new mysqli("localhost","kdhong","1234","kdhong_db");
	$db->set_charset("utf8");

	function mq($sql)
	{
		global $db;
		return $db->query($sql);
	}
?>
<?php
	include ('db_connect.php');
	if($_GET["userid"]){
		$uid = $_GET["userid"];
		$sql = mq("select * from user where id='".$uid."'");
		$member = $sql->fetch_array();
		if($member==0)
		{
	?>
		<div style='font-family:"malgun gothic"';><?php echo $uid; ?>는 사용가능한 아이디입니다.</div>
		<button value="닫기" onclick="id_check()">닫기</button>
	<?php
		}else{
	?>
		<div style='font-family:"malgun gothic"; color:red;'><?php echo $uid; ?>중복된아이디입니다.<div>
			<button value="닫기" onclick="window.close()">닫기</button>
	<?php
		}
	?>
	<?
	}
	elseif ($_GET["usernic"]) {
		$uid = $_GET["usernic"];
		$sql = mq("select * from user where nic_name='".$uid."'");
		$member = $sql->fetch_array();
		if($member==0)
		{
	?>
		<div style='font-family:"malgun gothic"';><?php echo $uid; ?>는 사용가능한 닉네임입니다.</div>
		<button value="닫기" onclick="nic_check()">닫기</button>
	<?php
		}else{
	?>
		<div style='font-family:"malgun gothic"; color:red;'><?php echo $uid; ?>중복된닉네임입니다.<div>
			<button value="닫기" onclick="window.close()">닫기</button>
	<?php
		}

	}
	?>
	<script>
	function id_check(){
		opener.document.getElementById("id_ch").value =1;
		window.close();
	}
	function nic_check(){
		opener.document.getElementById("nik_ch").value =1;
		window.close();
	}
	</script>
<?php
  include ('db_connect.php');
  $board_id = $_GET['board_id'];
?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>게시판</title>
    <link rel="stylesheet" type="text/css" href="css/style.css" />
    <style>
    #page_num {
      font-size: 14px;
      margin-left: 260px;
      margin-top:30px;
    }
    #page_num ul li {
      float: left;
      margin-left: 10px;
      text-align: center;
    }
    .fo_re {
      font-weight: bold;
      color:red;
    }
    </style>
    </head>
    <body>
    <div id="board_area">
      <?if($board_id=="board"){?>
      <h1>자유게시판</h1>
      <h4>자유롭게 글을 쓸 수 있는 게시판입니다.</h4>
      <div id="search_box">
        <form action="search_result.php" method="get">
          <select name="catgo">
            <option value="title">제목</option>
            <option value="name">글쓴이</option>
            <option value="content">내용</option>
          </select>
          <input type="text" name="search" size="40" required="required" /> <button>검색</button>
        </form>
      </div>
      <?}?>
      <?if($board_id=="notice"){?>
      <h1>공지사항</h1>
      <h4>공지사항 게시판입니다.</h4>
      <?}?>

        <table class="list-table">
          <thead>
              <tr>
                  <th width="70">번호</th>
                    <th width="500">제목</th>
                    <th width="120">글쓴이</th>
                    <th width="100">작성일</th>
                    <th width="100">조회수</th>
                </tr>
            </thead>
            <?php
              if(isset($_GET['page'])){
                $page = $_GET['page'];
              }
              else{
                $page = 1;
              }
              // 테이블에서 idx를 기준으로 내림차순해서 5개까지 표시
              $sql = mq("select * from $board_id");
              $row_num = mysqli_num_rows($sql); //게시판 총 레코드 수
              $list = 5; //한 페이지에 보여줄 개수
              $block_ct = 5; //블록당 보여줄 페이지 개수

              $block_num = ceil($page/$block_ct); // 현재 페이지 블록 구하기
              $block_start = (($block_num - 1) * $block_ct) + 1; // 블록의 시작번호
              $block_end = $block_start + $block_ct - 1; //블록 마지막 번호

              $total_page = ceil($row_num / $list); // 페이징한 페이지 수 구하기
              if($block_end > $total_page) $block_end = $total_page; //만약 블록의 마지박 번호가 페이지수보다 많다면 마지박번호는 페이지 수
              $total_block = ceil($total_page/$block_ct); //블럭 총 개수
              $start_num = ($page-1) * $list; //시작번호 (page-1)에서 $list를 곱한다.


              $sql2 = mq("select * from ".$board_id." order by idx desc limit $start_num, $list");
              while($board = $sql2->fetch_array()){
                  //title변수에 DB에서 가져온 title을 선택
                  $title=$board["title"];
                  if(strlen($title)>30)
                  {
                    //title이 30을 넘어서면 ...표시
                    $title=str_replace($board["title"],mb_substr($board["title"],0,30,"utf-8")."...",$board["title"]);
                  }
            ?>
          <tbody>
            <tr>
              <td width="70"><?php echo $board['idx']; ?></td>
              <td width="500"><a href="read.php?board_id=<?echo $board_id?>&idx=<?php echo $board["idx"];?>"><?php echo $title;?></a></td>
              <td width="120"><?php echo $board['name']?></td>
              <td width="100"><?php echo $board['date']?></td>
              <td width="100"><?php echo $board['hit']; ?></td>
            </tr>
          </tbody>
          <? } ?>
        </table>
        <!---페이징 넘버 --->
        <div id="page_num">
          <ul>
            <?php
              if($page <= 1)
              { //만약 page가 1보다 크거나 같다면
                echo "<li class='fo_re'>처음</li>"; //처음이라는 글자에 빨간색 표시
              }else{
                echo "<li><a href='?board_id=$board_id&page=1'>처음</a></li>"; //처음글자에 1번페이지로 갈 수있게 링크
              }
              if($page > 1)
              {
                $pre = $page-1;
                echo "<li><a href='?board_id=$board_id&page=$pre'>이전</a></li>";
              }
              for($i=$block_start; $i<=$block_end; $i++){
                //for문 반복문을 사용하여, 초기값을 블록의 시작번호를 조건으로 블록시작번호가 마지박블록보다 작거나 같을 때까지 $i를 반복시킨다
                if($page == $i){
                  echo "<li class='fo_re'>[$i]</li>"; //현재 페이지에 해당하는 번호에 굵은 빨간색을 적용
                }else{
                  echo "<li><a href='?board_id=$board_id&page=$i'>[$i]</a></li>";
              }
            }
              if($page < $total_page){
                $next = $page + 1;
                echo "<li><a href='?board_id=$board_id&page=$next'>다음</a></li>";
              }
              if($page >= $total_page){ //만약 page가 페이지수보다 크거나 같다면
                echo "<li class='fo_re'>마지막</li>"; //마지막 글자에 긁은 빨간색을 적용한다.
              }else{
                echo "<li><a href='?board_id=$board_id&page=$total_page'>마지막</a></li>"; //아니라면 마지막글자에 total_page를 링크한다.
              }

              ?>
          </ul>
        </div>
        <?
          if($board_id=="notice"){
            if($_SESSION['userid']=='admin'){
              ?>
              <div id="write_btn">
                <a href="write.php?board_id=<?echo $board_id;?>"><button>글쓰기</button></a>
              </div>
              <?
            }
          }
          else{
        ?>
        <div id="write_btn">
          <a href="write.php?board_id=<?echo $board_id;?>"><button>글쓰기</button></a>
        </div>
        <?
        }
        ?>
      </div>
    </body>
</html>
<?php
		include ('db_connect.php');
		$board_id = $_GET['board_id'];
    $bno = $_GET['idx'];
		$username = $_SESSION['userid'];
		$date = date("Y-m-d H:i:s");
    if($bno && $username && $_POST['content']){
        $sql = mq("insert into ".$board_id."_reply (con_num,id,name,content,date) values ('".$bno."','".$username."','".$username."','".$_POST['content']."','".$date."');");
        echo "<script>alert('댓글이 작성되었습니다.');
        location.href='read.php?board_id=$board_id&idx=$bno;';</script>";
    }else{
        echo "<script>alert('댓글 작성에 실패했습니다.');
        history.back();</script>";
    }

?>
<?
  include ('db_connect.php');
?>
<!DOCTYPE html>
<head>
<meta charset="UTF-8">
<title>게시판</title>
<link rel="stylesheet" type="text/css" href="css/style.css" />
</head>
<body>
<div id="board_area">

<?php

  /* 검색 변수 */
  $catagory = $_GET['catgo'];
  $search_con = $_GET['search'];
?>
  <h1><?php echo $catagory; ?>에서 '<?php echo $search_con; ?>'검색결과</h1>
  <h4 style="margin-top:30px;"><a href="board.php?board_id=board">홈으로</a></h4>
    <table class="list-table">
      <thead>
          <tr>
              <th width="70">번호</th>
                <th width="500">제목</th>
                <th width="120">글쓴이</th>
                <th width="100">작성일</th>
                <th width="100">조회수</th>
            </tr>
        </thead>
        <?php
          $sql2 = mq("select * from board where $catagory like '%$search_con%' order by idx desc");
          while($board = $sql2->fetch_array()){

          $title=$board["title"];
            if(strlen($title)>30)
              {
                $title=str_replace($board["title"],mb_substr($board["title"],0,30,"utf-8")."...",$board["title"]);
              }
        ?>
      <tbody>
        <tr>
          <td width="70"><?php echo $board['idx']; ?></td>
          <td width="500">

        <a href='read.php?board_id=board&idx=<?php echo $board["idx"]; ?>'><span style="background:yellow;"><?php echo $title; ?></span></a></td>
          <td width="120"><?php echo $board['name']?></td>
          <td width="100"><?php echo $board['date']?></td>
          <td width="100"><?php echo $board['hit']; ?></td>

        </tr>
      </tbody>

      <?php } ?>
    </table>

    <div id="search_box2">
      <form action="search_result.php" method="get">
      <select name="catgo">
        <option value="title">제목</option>
        <option value="name">글쓴이</option>
        <option value="content">내용</option>
      </select>
      <input type="text" name="search" size="40" required="required"/> <button>검색</button>
    </form>
  </div>
</div>
</body>
</html>
<?
  include ('db_connect.php');
  $id = $_POST['id'];
  $nic = $_POST['name'];
  $pw = $_POST['passw'];
  $date=date("Y-m-d");
  $sql = mq("insert into user (id, nic_name, pw, user_date) values ('".$id."','".$nic."','".$pw."','".$date."')");
  echo "
  <script>
    location.href='index.php'
  </script>";
?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>회원가입</title>
    <style media="screen">
    <style>
      * {margin: 0; padding: 0;}
      #sign_up_box{
        width:400px;
        height:150px;
        border:solid 2px gray;
        position: absolute;
        left: 50%; top: 50%;
        margin-left: -200px;
        margin-top: -100px;
        background-color: orange;
      }
      #sign_up_button{
        position: absolute;
        left: 40%; top: 80%;
      }

    </style>
    <script>
      function check_id(){
        var userid = document.getElementById("uid").value;
        if(userid)
	       {
		         url = "check.php?userid="+userid;
			       window.open(url,"chkid","width=300,height=100");
		     }
         else{
			        alert("아이디를 입력하세요");
		          }
	        }

      function check_nik(){
        var usernic = document.getElementById("nic").value;
        if(usernic)
         {
             url = "check.php?usernic="+usernic;
             window.open(url,"chkid","width=300,height=100");
         }
         else{
              alert("닉네임을 입력하세요");
              }
          }

      function passwordCheck(){
          var pw = document.getElementById("pw").value;
          var pw_ck = document.getElementById("pw_ck").value;
          var id_ch = document.getElementById("id_ch").value;
          var nik_ch = document.getElementById("nik_ch").value;
          if (pw=="")  {
            alert("비밀번호를 입력해주세요.");
          }
          else if(id_ch==0){
            alert("아이디 중복확인을 해주세요");
          }
          else if(nik_ch==0){
            alert("닉네임 중복확인을 해주세요");
          }
          else if(pw != pw_ck){
            alert("비밀번호가 일치하지 않습니다 확인해 주세요.");
          }
          else{
            document.getElementById("sign").submit();

          }
      }
    </script>
  </head>
  <body>
    <div id="sign_up_box">


    <form class="" action="sign_up_search.php" method="post" id="sign">
      <table>
        <tr>
          <td>아이디</td>
          <td>
            <input type="text" name="id" id="uid">
          </td>
          <td>
            <button type="button" name="button" onclick="check_id()">중복확인</button>
            <input type="hidden" id="id_ch" name="" value="0">
          </td>
        </tr>
        <tr>
          <td>닉네임</td>
          <td>
            <input type="text" name="name" id="nic">
          </td>
          <td>
            <button type="button" name="button" onclick="check_nik()">중복확인</button>
            <input type="hidden" id="nik_ch" name="" value="0">
          </td>
        </tr>
        <tr>
          <td>비밀번호</td>
          <td>
            <input type="password" id="pw" name="passw">
          </td>
        </tr>
        <tr>
          <td>비밀번호 확인</td>
          <td>
            <input type="password" id="pw_ck" name="pass_check">
          </td>
        </tr>
      </table>
      <button id="sign_up_button" type="button" name="button" align="right" onclick="passwordCheck()">회원가입</button>

    </form>
    </div>
  </<?php

  include ('db_connect.php');


  $username = $_SESSION['userid'];
  $usernic = $_SESSION['user_nic'];
  $board_id = $_GET['board_id'];
  $title = $_POST['title'];
  $content = $_POST['content'];
  $date = date('Y-m-d');

  $upload_dir = 'uploads/';
  $upload_file = $upload_dir . $_FILES['SelectFile']['name'];

  $file_name = iconv("utf-8","CP949",$upload_file);
  echo "<script>
  alert('$tmpfile');
  </script>";
  print "<pre>";
  if (move_uploaded_file($_FILES['SelectFile']['tmp_name'], $file_name)) {
    print "[수신한 내용]<br><br>";
    print "PATH: " .$upload_file."<br>";
    print "제목 : ".$_POST['title']."<br>";
    print "내용 : ".$_POST['content']."<br>";
    print "파일 :".$_FILES['SelectFile']['type']."<br>";
    if($_FILES['SelectFile']['type']=="image/jpeg"||$_FILES['SelectFile']['type']=="image/gif"){
      print "<img src=$upload_file width='300'>";
    }
  }
  else {
    print "파일 업로드  실패 : ";
    switch ($_FILES[SelectFile][error]) {
      case UPLOAD_ERR_INI_SIZE:
        print "php.ini 파일의 upload_max_filesize(".ini_get("upload_max_filesize").")보다 큽니다.<br>";
      break;
      case UPLOAD_ERR_FORM_SIZE:
        print "업로드 한 파일이 Form의 MAX_FILE_SIZE 값보다 큽니다.<br>";
      break;
      case UPLOAD_ERR_PARTIAL:
        print "파일의 일부분만 전송되었습니다.<br>";
      break;
      case UPLOAD_ERR_NO_FILE:
        print "파일이 전송되지 않았습니다.<br>";
      break;
      case UPLOAD_ERR_NO_TMP_DIR:
        print "임시 디렉토리가 없습니다.<br>";
      break;
    }
    print_r($_FILES);
  }
  print "</pre>";

  if($username && $title && $content){
      $sql = mq("insert into ".$board_id."(id,name,title,content,date,file) values('".$username."','".$usernic."','".$title."','".$content."','".$date."','".$_FILES['SelectFile']['name']."');");
      echo "<script>
      alert('글쓰기 완료되었습니다.');
      location.href='board.php?board_id=$board_id';</script>";
    }
    else{
      echo "<script>
      alert('글쓰기에 실패했습니다.');
      history.back();</script>";
    }
?>
body>
<!DOCTYPE html>
<head>
<meta charset="UTF-8">
<title>게시판</title>
<style>
  #board_write {
    width:900px;
    position:relative;
    margin:0 auto;
  }
  #write_area {
    margin-top:70px;
    font-size:14px;
  }
  #in_title {
    margin-top:30px;
  }
  #in_title textarea {
    font-weight:bold;
    font-size:26px;
    color:#333;
    width: 900px;
    border:none;
    resize: none;
  }
  .wi_line {
    border:solid 1px lightgray;
    margin-top:10px;
  }
  #in_content {
    margin-top:10px;
  }
  #in_content textarea {
    font:14px;
    color:#333;
    width: 900px;
    height: 400px;
    resize: none;
  }
  .bt_se {
    margin-top:20px;
    text-align:center;
  }
  .bt_se button {
    width:100px;
    height:30px;
  }
</style>
</head>
<body>
  <?$board_id=$_GET['board_id'];?>
    <div id="board_write">
        <h4>글을 작성하는 공간입니다.</h4>
            <div id="write_area">
                <form enctype="multipart/form-data" action="write_ok.php?board_id=<?echo $board_id;?>" method="post">
                    <div id="in_title">
                        <textarea name="title" id="utitle" rows="1" cols="55" placeholder="제목" maxlength="100" required></textarea>
                    </div>

                    <div class="wi_line"></div>
                    <div id="in_content">
                        <textarea name="content" id="ucontent" placeholder="내용" required></textarea>
                    </div>

                      <input type="file" name="SelectFile" />


                    <div class="bt_se">
                        <button type="submit">글 작성</button>
                    </div>
                </form>
            </div>
        </div>
    </body>
</html>
![bbt_setting](https://github.com/user-attachments/assets/13b02ebf-b371-41c6-8674-bc48798482b2)
![ÁĶļņ ūøĀ―](https://github.com/user-attachments/assets/06f043bf-05a0-4129-831c-7fd3747a05e9)

</html>
