# hello-world
for fun
that's interesting. my big project is to create an artificial intelligence.
Recently, I wrote a simplified Chinese chess.
/*************

establish a simplified Chinese chess
*************/



/****************/
```javascript
var qizi={
ju:"\u{8eca}",
ma:"\u{99ac}",
xiang:"\u{76f8}",
shi:"\u{58eb}",
shuai:"\u{5e25}",
pao:"\u{70ae}",
zu:"\u{5352}",
bing:"\u{5175}",
jiang:"\u{5c07}",
xang:"\u{8c61}",
si:"\u{4ed5}",
test:""};
```
/*****************
      draw 9by10 "div"s stand for chessboard.
RC is an array, of which the index number  indicate the chessboard coordinat. The index number range from 0 to 89. The elements of RC are arrays, which indicate the coordinate of chessboard in the form of (row,collumn), for example,0=> (0,0),15=>(1,5),89=>(8,9) etc.
********************/
```javascript
var RC=[];
var chessboard=document.createElement("div");
chessboard.style=`display:block;
position:absolute;
left:10;
top:30px;
transform:rotate(180deg);
width:340px;
height:400px;
border-radius:30px;
background-color:pink`;
document.body.appendChild(chessboard);

for(var i=0;i<9;i++){
     for(var j=0;j<10;j++){
           RC.push([i,j]);
     }
}

var container=
document.createDocumentFragment();
```
/************************************
    draw chessboard
************************************/
```javascript
var selectMode=true;

RC.forEach(function(x,index){
       var div=document.createElement("div");
        div.id=index;
        div.innerHTML="\u{256c}";
        div.style=`display:block;
position:absolute;
top:${x[1]*37}px;
left:${x[0]*37}px;
color:cyan;
font-size:32px;

transform: rotate(180deg);
width:35px;
height:35px;
padding: auto;
border:2px pink solid;
border-radius:100%;
background-color:pink;
margin:10px 5px;
overflow:visible`;


      div.onclick=function(){
       if(selectMode){
           selectMode=false;
           refresh();
           active(this);
            canReachDirect(this.id);
            threateningAlert(this.id);
            guarding(this.id);
            protected(this.id);
           log("select"+select(this.id));
       }else{
          highlight(this);
          var act=actived();
          if(act){
         move(act.id, this.id);
         log(RC[act.id]+"should move to"+RC[this.id]);
         }
         refresh();
         selectMode=true;
          inactivate();
         
       }
};
        container.appendChild(div);
});


   chessboard.appendChild(container);
   
```
/***************************************
    show different appearance between actived and inactived divs.
***************************************/ function active(DIV){
     DIV.style.borderColor="green";
     DIV.setAttribute("class","active");
   }  

function actived(){
     return document.querySelector(".active");
};

function inactivate(){
   if(actived()){
    actived().style.borderColor="pink";
    actived().removeAttribute("class");}
}

function highlight(DIV){
   var h= document.querySelector(".high");
     if(h){
          h.style.borderColor="pink";
          h.removeAttribute("class");
      }
     DIV.style.borderColor="red";
     DIV.setAttribute("class", "high");
}


function log(x){
    log1.innerHTML=x;
}

function Alog(x){
    log2.innerHTML=x;
}

var log1=document.body.appendChild(document.createElement("output"));
log1.style="position:absolute;top:0px;left:0;color:blue";
log1.innerHTML="simplified Chinese chess";

var log2=document.body.appendChild(document.createElement("output"));
log2.style="position:absolute;top:430px;
left:10;color: brown;width:250px;height:auto;background-color:lime";
log2.innerHTML="
selected:green circle<br>"+
"protected by green colored pieces<br>"+
"is guarding purple pieces<br>"+
"can reach: blue colored<br>"+
"threatened by yellow colored<br>"+
"last selected colored by red circle<br>"+
"for more information,click";
log2.onclick=function(){
     alert("temporary can do nothing");
};
/****************************************
    define a chess object.
****************************************/
  function chess(name, r, c, flag){
      this.name=name;
      this.x=r;
      this.y=c;
      this.flag=flag;
}

function Chess(name,pos,flag){
    return  new chess(name, RC[pos][0], RC[pos][1], flag);
}


chess.prototype.draw=function(){
     var div=document.getElementById(this.x*10+this.y);
div.innerHTML=qizi[this.name];
div.style.color=this.flag;
};

chess.prototype.toString=function(){
    return this.flag+qizi[this.name]+this.x+this.y;
};
/************************************
     layout: an array of all pieces on the chessboard.
***********************************/
    var layout=[];
     layout.push(
Chess("ju",0,"red"),
Chess("ma",10,"red"),
Chess("xiang",20,"red"),
Chess("shi",30,"red"),
Chess("jiang",40,"red"),
Chess("ju",80,"red"),
Chess("ma",70,"red"),
Chess("xiang",60,"red"),
Chess("shi",50,"red"),
Chess("pao",12,"red"),
Chess("pao",72,"red"),
Chess("zu",3,"red"),
Chess("zu",23,"red"),
Chess("zu",43,"red"),
Chess("zu",63,"red"),
Chess("zu",83,"red"),
Chess("ju",9,"black"),
Chess("ma",19,"black"),
Chess("xang",29,"black"),
Chess("si",39,"black"),
Chess("jiang",49,"black"),
Chess("ju",89,"black"),
Chess("ma",79,"black"),
Chess("xang",69,"black"),
Chess("si",59,"black"),
Chess("pao",17,"black"),
Chess("pao",77,"black"),
Chess("bing",6,"black"),
Chess("bing",26,"black"),
Chess("bing",46,"black"),
Chess("bing",66,"black"),
Chess("bing",86,"black")
);

function refresh(){
/*****************/
     RC.forEach(function(x,i){
        var div=document.getElementById(i);
        div.innerHTML="\u{256c}";
        div.style.color="gray";
});
/****************/
     layout.forEach(x=>x.draw());
}

function select(id){
     return layout.find(f=>f.x==RC[id][0]&&f.y==RC[id][1]);
}

function remove(t){
    if(t.name=="test")return;
     var index=layout.indexOf(t);
     layout.splice(index,1);
}

function move(fid, tid){
       var f=select(fid), t=select(tid);
        if(f){
          
       if(!t)t=Chess("test",tid,"green");
       if(rule(f,t)){     
            if(t){
              remove(t);
              f.x=t.x;
              f.y=t.y;
             }else{
             f.x=RC[tid][0];
             f.y=RC[tid][1];
             }
        }

      }
         
     refresh();
}

/*****************************************
       test area
*****************************************/

 
  refresh();






/*****************************************
  Here is the rules of moving pieces.
  The function rule(from, to) judges whether
the piece can go to the place from the origin.

******************************************/
function rule(f, t){
    if(partner(f,t))return false;
    if(f.name=="ju")return ruleju(f,t);
    if(f.name=="ma")return rulema(f,t);
    if(/xi?ang/.test(f.name))return rulexiang(f,t);
    if(/sh?i/.test(f.name))return ruleshi(f,t);
    if(/jiang|shuai/.test(f.name))return rulejiang(f,t);
    if(/pao/.test(f.name))return rulepao(f,t);
    if(/bing|zu/.test(f.name))return rulezu(f,t);
    return true;
}

function grule(f, t){
    if(partner(f,t)){

    if(f.name=="ju")return ruleju(f,t);
    if(f.name=="ma")return rulema(f,t);
    if(/xi?ang/.test(f.name))return rulexiang(f,t);
    if(/sh?i/.test(f.name))return ruleshi(f,t);
    if(/jiang|shuai/.test(f.name))return rulejiang(f,t);
    if(/pao/.test(f.name))return rulepao(f,t);
    if(/bing|zu/.test(f.name))return rulezu(f,t);
     }
    return false;
}

/**********************/
function partner(f,t){
    if(t){
    return f.flag==t.flag;}
    return false;
}
/***************************/
function ruleju(f,t){
  //  alert("ruleju");
    var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);

    if(f.x==t.x&&!layout.some(
        b=>b.x==f.x&&b.y>minY&&b.y<maxY
)){return true;}
    if(f.y==t.y&&!layout.some(
        b=>b.y==f.y&&b.x>minX&&b.x<maxX
)){return true;}
    return false;
}

/*************************/
    function rulema(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);

if(
(maxX-minX)==1&&(maxY-minY)==2&&!layout.some(b=>b.x==f.x&&b.y==(maxY+minY)/2)
){return true;}
if(
(maxX-minX)==2&&(maxY-minY)==1&&!layout.some(b=>b.y==f.y&&b.x==(maxX+minX)/2)
){return true;}

return false;
}

/*********************/

function rulexiang(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);
if(f.flag=="red"&&t.y>4)return false;
if(f.flag=="black"&&t.y<5)return false;
if(
(maxX-minX)==2&&(maxY-minY)==2&&!layout.some(b=>b.x==(maxX+minX)/2&&b.y==(maxY+minY)/2)
){return true;}

return false;
}

/*********************/
function ruleshi(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);
if(f.flag=="red"&&t.y>2)return false;
if(f.flag=="black"&&t.y<7)return false;
if(
(maxX-minX)==1&&(maxY-minY)==1&&t.x>2&&t.x<6
){return true;}

return false;
}

/*********************/

function rulejiang(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);
if(f.flag=="red"&&t.y>2)return false;
if(f.flag=="black"&&t.y<7)return false;
if(
(maxX-minX)==0&&(maxY-minY)==1&&t.x>2&&t.x<6
){return true;}
if(
(maxX-minX)==1&&(maxY-minY)==0&&t.x>2&&t.x<6
){return true;}
return false;
}

/*********************/

function rulezu(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);
if(f.flag=="red"&&t.y<f.y){return false;}
if(f.flag=="black"&&t.y>f.y){return false;}
if(f.flag=="red"&&t.y<5&&t.x!=f.x){return false;}
if(f.flag=="black"&&t.y>4&&t.x!=f.x){return false;}
if(
(maxX-minX)==0&&(maxY-minY)==1
){return true;}
if(
(maxX-minX)==1&&(maxY-minY)==0
){return true;}
return false;
}

/*********************/

function rulepao(f,t){
var maxX=Math.max(f.x,t.x),
          minX=Math.min(f.x,t.x),
          maxY=Math.max(f.y,t.y),
          minY=Math.min(f.y,t.y);

if(t.name=="test")return ruleju(f,t);

if(t.x==f.x&&layout.filter(
    b=>b.x==f.x&&b.y>minY&&b.y<maxY
).length==1){return true;}

if(t.y==f.y&&layout.filter(
    b=>b.y==f.y&&b.x>minX&&b.x<maxX
).length==1){return true;}

return false;
}

/********************/
/*****************
       define AI: AI(flag), for flags(red or black).
     AI have strategy, which determined defend or attack.
      first step: AI is evoked by the flag.
      second: AI analyse the situation, get an idea to defend or attack.
      third: AI try to defend. first choose the most important piece to defend. then try different ways to defend it: escape, kill, block and guard.
if all ways turn to be useless, then turn to defend the second important one, and so on.

if any defend success, the defend function return true, otherwise,return false.

       forth: AI try to attack. first choose the most important one to attack. sorts of attack include: kill, threaten, block, or just move.

    AI can try to test a way of defence or attack by random, to see whether it improve the situation or not.
****************/
      function AI(flag){
          if(this==window)return new AI(flag);
          this.flag=flag;
          this.inDanger=[];
          this.score=0;
          this.policy="";
}

AI.prototype.strategy=function(){};
AI.prototype.analyze=function(){};
AI.prototype.defend=function(){};
AI.prototype.attack=function(){};
AI.prototype.test=function(){};
/************************************

 Add AI player.
       ①canReach: list the available points on the chessboard(represented as the points's id's)
a selected piece can reach.
       ②canReachDirect: show the available Points on the chessboard.
③threatening: return the enemies attacking the piece.
       ③guardingList: return the allies guarding
the selected piece.
       ④protectedList.

AI player follow the rules:
     defence rules: escape(d), kill(d), gurad(d),black(d).
************************************/
function canReach(id){
      var f=select(id);
       if(f){
               boardPoints=Object.keys(RC);
              return boardPoints.filter(
                 function(x){
                       var t=select(x);
                       if(!t)t=Chess("test",x,"green");
                       return rule(f,t);
                  }
               );
       }else{
       return [];}
}

function canReachDirect(id){
    // alert(canReach(id));
     canReach(id).forEach(
     function(x){
       var div=document.getElementById(x);
        div.style.color="blue";
      }
     );
}

/************************/

 function threatening(id){
var t=select(id);
       if(t){
          return layout.filter(x=>rule(x,t));
       }else{
       return [];}
}

function threateningAlert(id){
     threatening(id).forEach(
      function(t){
          var div=document.getElementById(t.x*10+t.y);
         div.style.color="yellow";
}
);
}

function guardingList(id){
      var g=select(id);
       if(g){
          return layout.filter(x=>grule(g,x));
       }else{
       return [];}
}

function protectedList(id){
      var t=select(id);
       if(t){
          return layout.filter(x=>grule(x,t));
       }else{
       return [];}
}

function guarding(id){
guardingList(id).forEach(
      function(t){
          var div=document.getElementById(t.x*10+t.y);
         div.style.color="purple";
}
);
}


function protected(id){
protectedList(id).forEach(
      function(t){
          var div=document.getElementById(t.x*10+t.y);
         div.style.color="green";
}
);
}




/******************************************
      stylesheet area

*****************************************/

   var styleObject=document.createElement("style");
document.body.appendChild(styleObject);
var stylesheet=styleObject.sheet;

var styleRule=`p{background-color:teal;}`;


stylesheet.insertRule(styleRule, 0);












































