## faceApi를 이용한 미용프로그램의 구현 - 15팀

### 팀 구성

- 팀장 : [컴퓨터공학과] 20191007 최하람
- 팀원 : [컴퓨터공학과] 20191095 서정목, 20191108 최연우, 20191112 허영준

### 코드

```javascript
let img, parts;
let options = {withLandmarks: true, withDescriptors: false};
let r = [];
let g = [];
let b = [];

let q = [];
let w = [];
let e = [];

let slider, val = 0, temp = 1;
let button1, button2, button3;
let b1 = 1, b2 = 0, b3 = 0;

function preload() {
  img = loadImage('face.png');
}

function setup() {
  createCanvas(img.width*2, img.height*2);
  faceapi = ml5.faceApi(options, modelReady);
  slider = createSlider(0, 10, 1);
  slider.position(width/2, height/2);
  button1 = createButton('눈썹');
  button1.position(width/2, height/2+30);
  button2 = createButton('눈');
  button2.position(width/2, height/2+60);
  button3 = createButton('입술');
  button3.position(width/2, height/2+90);
  image(img, img.width, 0);  
  image(img,0,0);
  for(let i=0;i<3;i++)
  {
    r[i] = [];
    g[i] = [];  
    b[i] = [];
  }
  r[0][0] = 255;
  g[0][0] = 0;
  b[0][0] = 0;
  
  r[0][1] = 0;
  g[0][1] = 255;
  b[0][1] = 0;
  
  r[0][2] = 0;
  g[0][2] = 0;
  b[0][2] = 255;
  
  r[1][0] = 190;
  g[1][0] = 0;
  b[1][0] = 255;
  
  r[1][1] = 45;
  g[1][1] = 197;
  b[1][1] = 244;
  
  r[1][2] = 255;
  g[1][2] = 204;
  b[1][2] = 0;
  
  r[2][0] = 0;
  g[2][0] = 255;
  b[2][0] = 255;
  
  r[2][1] = 255;
  g[2][1] = 0;
  b[2][1] = 255;
  
  r[2][2] = 255;
  g[2][2] = 150;
  b[2][2] = 0;
}

function draw(){
  Eyebraw();
  colorbox();
  button1.mousePressed(BUTTON1);
  button2.mousePressed(BUTTON2);
  button3.mousePressed(BUTTON3);
}

function Eyebraw(){
  val = slider.value();
  if(val != temp){
    background(255);
    image(img, img.width, 0);
    image(img,0,0);
    drawFace();
  }
  temp = val;
}

function BUTTON1(){
  b1 = 1;
  b2 = 0, b3 = 0;
}
function BUTTON2(){
  b2 = 1;
  b1 = 0, b3 = 0;
}
function BUTTON3(){
  b3 = 1;
  b1 = 0, b2 = 0;
}

// 색상 지정 표
function colorbox(){
  stroke(0);
  strokeWeight(1);
  for(let i=0;i<3;i++){
    for(let j=0;j<3;j++){
      if(mouseX>2+90*i&&mouseX<2+90*(i+1)&&mouseY>img.height+90*j&&mouseY<img.height+90*(j+1)){
      if(mouseIsPressed){
        if(b1 == 1){
          braw(r[i][j],g[i][j],b[i][j]);
          q[0]=r[i][j],w[0]=g[i][j],e[0]=b[i][j];
        }
        else if(b2 == 1){
          eyes(r[i][j],g[i][j],b[i][j]);
          q[1]=r[i][j],w[1]=g[i][j],e[1]=b[i][j];
        }
        else if(b3 == 1){
          mouse(r[i][j],g[i][j],b[i][j]);
          q[2]=r[i][j],w[2]=g[i][j],e[2]=b[i][j];
        }
        
      }
      fill(r[i][j]-20,g[i][j]-20,b[i][j]-20);
      rect(2+90*i,img.height+90*j,90);
      }
      else {
        fill(r[i][j],g[i][j],b[i][j]);
        rect(2+90*i,img.height+90*j,90);
      } 
    }
  }
}

function modelReady() {
  faceapi.detectSingle(img, gotResults);
}

function gotResults(err, results) {
  if (err) {
    console.error(err);
    return;
  }
  console.log(results);
  parts = results.parts;
  noFill();
  stroke(0);
  strokeWeight(1);
  drawFace();
}

function drawFace() {
  mouse(q[2],w[2],e[2]);
  eyes(q[1],w[1],e[1]);
  braw(q[0],w[0],e[0]);
  //코 만들기
  noFill();
  stroke(0);
  beginShape();
  for(let i=0; i<parts.nose.length; i++){
    vertex(parts.nose[i]._x, parts.nose[i]._y);
  }
  endShape();
}

//입술 만들기
function mouse(A,B,C){
  fill(A,B,C);
  beginShape();
  for(let i=0; i<7; i++){
    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
  }
  for(let i=16; i>11; i--){
    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
  }
  vertex(parts.mouth[0]._x, parts.mouth[0]._y);
  endShape();
  beginShape();
  for(let i=6; i<13; i++){
    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
  }
  for(let i=19; i>15; i--){
    vertex(parts.mouth[i]._x, parts.mouth[i]._y);
  }
  vertex(parts.mouth[6]._x, parts.mouth[6]._y);
  endShape();
}

//눈 만들기
function eyes(A,B,C){
  fill(A,B,C);
  beginShape();
  for(let i=0; i<parts.leftEye.length; i++){
    vertex(parts.leftEye[i]._x, parts.leftEye[i]._y);
  }
  vertex(parts.leftEye[0]._x, parts.leftEye[0]._y);
  endShape();

  beginShape();
  for(let i=0; i<parts.rightEye.length; i++){
    vertex(parts.rightEye[i]._x, parts.rightEye[i]._y);
  }
  vertex(parts.rightEye[0]._x, parts.rightEye[0]._y);
  endShape();
}

//눈썹 만들기
function braw(A,B,C){    
  noFill();
  stroke(A,B,C);
  strokeWeight(val);
  beginShape();
  for(let i=0; i<parts.leftEyeBrow.length; i++){
    vertex(parts.leftEyeBrow[i]._x, parts.leftEyeBrow[i]._y);
  }
  endShape();

  beginShape();
  for(let i=0; i<parts.rightEyeBrow.length; i++){
    vertex(parts.rightEyeBrow[i]._x, parts.rightEyeBrow[i]._y);
  }
  endShape();
  strokeWeight(1);
}
```

## 실행결과


https://user-images.githubusercontent.com/62204475/168416323-e0bdb530-6188-4a44-b4bf-739d07285f6f.mov


## 소감
 - 최하람 : 혼자 하였다면 오래 걸렸을 프로젝트를 훌룡한 팀원과 함께하니 아이디어 구상과 코드 구현하는 시간이 많이 줄어서 좋았습니다. 눈썹, 눈 , 입술을 따로 선택하여 변형할 수 있게 하려고 하는 도중 슬라이더를 이용할 때 이미지가 사라지는 오류가 발생하여 혼재 해결해보려고 하였지만 불가능하여 막막하였습니다. 어쩔 수 없이 이 문제를 팀원들에게 공유하였는데 생각보다 금방 해결되었고 다음 단계로 진행할 수 있었습니다. 
 이 프로그램을 실제 시장에서 쓰기에는 아직 무리가 있지만 얼굴을 인식하고 가상으로 화장 시켜주는 프로그램이 어떻게 구성되었는지 알 수 있엇던 좋은 시간이었습니다.
 - 최연우 : 혼자 코딩을 했었다면 입술을 연결하는 방법을 생각하는 것이 힘들었을 것 같고 또 시간도 많이 걸렸을 텐데 서로 코딩할 부분을 나누고 모르겠는 부분은 어떻게 구현할지 함께 생각하면서 코드를 짜니까 시간도 아끼고 더 효율적이었던 것 같습니다. 같이 하는 것의 필요성을 깨닫는 시간이었습니다.
 - 허영준 : face api를 이용해 사진에서 눈, 코, 입 등 얼굴의 구성요소를 인식해 점으로 표현한다는 점이 새로워서 즐겁게 학습할 수 있었습니다. 또, 팀원들과 아이디어를 주고받으며 사고방식의 관점을 넓힐 수 있었고, 의견 조율 또한 막힘없이 진행되어 많은 것을 배웠다고 생각합니다.
 - 서정목 : 각자 파트를 나눠서 진행하니 프로그래밍하는 시간을 효율적으로 단축시킬수 있었습니다. 또한 각자 짠 코드를 팀원들과 분석하여 단축시킬 방법을 함께 생각해보며 코드를 혼자 만들었을때 보다 간략화 시킬수 있었던것 같습니다. 함께 진행하니 여러가지 아이디어가 나오면서 조금더 멋진 결과가 나와 즐거운 시간이었습니다.
