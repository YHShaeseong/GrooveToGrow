# Groove to Grow 🕺💃
> AI 기반 아동 홈트레이닝 플랫폼

![Groove to Grow Logo](Title.png)

## 📖 프로젝트 개요

**Groove to Grow**는 MediaPipe AI 기술을 활용하여 아이들이 집에서도 재미있게 운동할 수 있도록 돕는 실시간 동작 인식 홈트레이닝 플랫폼입니다. 줌바 동작을 따라하며 아이들의 신체 발달과 건강한 성장을 지원합니다.

## ✨ 핵심 기능

### 1. 🎥 실시간 동작 인식
- **MediaPipe Pose** 활용한 실시간 자세 분석
- 웹캠을 통한 사용자 동작 감지 및 시각화
- 33개 주요 관절 포인트 추적

### 2. 🎵 인터랙티브 줌바 가이드
- 가이드 영상과 사용자 동작 실시간 비교
- 5초 카운트다운으로 준비 시간 제공
- 동작 정확도 기반 점수 시스템

### 3. 📊 운동 성과 분석
- 아이들을 위한 직관적인 점수 표시 (70-100점)
- 부모님을 위한 상세 운동 리포트
- 운동 시간, 소모 칼로리, 운동 강도 측정

### 4. 🛡️ 개인정보 보호
- GDPR 준수 개인정보 동의 시스템
- 아동 데이터 보호 강화
- 로컬 처리로 데이터 안전성 보장

## 🏗️ 기술 스택

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **AI/ML**: MediaPipe Pose Detection
- **웹캠 처리**: WebRTC getUserMedia API
- **배포**: Netlify
- **버전 관리**: Git, GitHub

## 📁 프로젝트 구조

```
GrooveToGrow/
├── index.html              # 메인 애플리케이션 파일
├── groove-to-grow.html     # 백업용 HTML 파일
├── Title.png               # 프로젝트 로고
├── Zumba.mp4              # 가이드 영상
├── _redirects             # Netlify SPA 라우팅 설정
└── README.md              # 프로젝트 문서
```

## 🔧 주요 코드 구조 분석

### 1. MediaPipe 초기화 (1057-1101줄)
```javascript
async initializeMediaPipe() {
  // 사용자 카메라용 Pose 인스턴스 생성
  this.userPose = new Pose({
    locateFile: (file) => `https://cdn.jsdelivr.net/npm/@mediapipe/pose/${file}`
  });

  // 줌바 영상용 Pose 인스턴스 생성
  this.zumbaPose = new Pose({...});
}
```
**목적**: 실시간 동작 인식을 위한 MediaPipe 라이브러리 초기화

### 2. 실시간 카메라 처리 (1109-1148줄)
```javascript
renderMainCamera(results) {
  // 캔버스에 비디오 프레임 그리기
  this.canvasCtx.drawImage(results.image, 0, 0, width, height);

  // 포즈 랜드마크 시각화
  if (results.poseLandmarks) {
    drawConnectors(this.canvasCtx, results.poseLandmarks, POSE_CONNECTIONS);
    drawLandmarks(this.canvasCtx, results.poseLandmarks);
  }
}
```
**목적**: 사용자의 실시간 동작을 캔버스에 시각화하고 골격 구조 표시

### 3. 줌바 가이드 영상 처리 (1410-1428줄)
```javascript
async processZumbaVideo() {
  // 영상 프레임을 MediaPipe로 전송
  await this.zumbaPose.send({ image: this.zumbaVideo });

  // 다음 프레임 처리를 위한 재귀 호출
  if (!this.zumbaVideo.paused) {
    requestAnimationFrame(() => this.processZumbaVideo());
  }
}
```
**목적**: 가이드 영상의 각 프레임에서 포즈를 추출하여 사용자와 비교

### 4. 운동 성과 추적 (1479-1526줄)
```javascript
startExerciseTracking() {
  this.exerciseStartTime = Date.now();
  this.exerciseTimer = setInterval(() => {
    this.updateExerciseStats();
  }, 1000);
}

updateExerciseStats() {
  const minutes = Math.floor(currentExerciseTime / 60);
  const totalCalories = Math.round((currentExerciseTime / 60) * 10);
  // UI 업데이트
}
```
**목적**: 운동 시간과 소모 칼로리를 실시간으로 계산하고 표시

### 5. 점수 시스템 (1528-1553줄)
```javascript
updateKidsScore(score) {
  if (score >= 90) {
    message = '와! 정말 완벽해요! 🌟';
  } else if (score >= 80) {
    message = '정말 잘했어요! 👏';
  }
  // 점수에 따른 격려 메시지 표시
}
```
**목적**: 아이들의 동기부여를 위한 점수 기반 피드백 시스템

## 🎨 UI/UX 특징

### 반응형 디자인 (627-660줄)
```css
@media (max-width: 768px) {
  .main-container {
    flex-direction: column;
  }
}
```
**목적**: 모바일 기기에서도 최적화된 사용자 경험 제공

### 아동 친화적 디자인 (468-532줄)
```css
.big-score-display {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 50px;
  border-radius: 25px;
}

.score-value {
  font-size: 6em;
  color: #ffd700;
  animation: scoreGlow 2s ease-in-out infinite alternate;
}
```
**목적**: 큰 글씨와 화려한 애니메이션으로 아이들의 흥미 유발

## 🚀 설치 및 실행

### 로컬 실행
```bash
# 프로젝트 클론
git clone https://github.com/YHShaeseong/GrooveToGrow.git

# 디렉토리 이동
cd GrooveToGrow

# 로컬 서버 실행 (Python 3)
python -m http.server 8000

# 브라우저에서 접속
open http://localhost:8000
```

### 온라인 데모
🌐 **라이브 데모**: [groovetogrow.netlify.app](https://groovetogrow.netlify.app)

## 🎯 타겟 사용자

- **주요 사용자**: 만 5-12세 아동
- **보조 사용자**: 부모님 (운동 성과 모니터링)
- **사용 환경**: 집, 키즈카페, 어린이집 등

## 🏆 기대 효과

1. **신체 발달**: 균형감각과 협응력 향상
2. **건강 습관**: 어린 시절부터 운동 습관 형성
3. **재미와 학습**: 게임화된 운동으로 지속적인 참여 유도
4. **안전성**: 집에서 안전하게 할 수 있는 운동 환경 제공

## 🛠️ 기술적 특징

### 성능 최적화
- **실시간 처리**: 60fps 영상 처리 최적화
- **메모리 관리**: 효율적인 캔버스 렌더링
- **브라우저 호환성**: 최신 브라우저 지원

### 보안 및 개인정보
```javascript
// 개인정보 동의 체크 (1569-1578줄)
handlePrivacyCheckbox() {
  const isChecked = this.privacyCheckbox.checked;
  this.privacyConfirmBtn.disabled = !isChecked;
}
```
**목적**: 아동 데이터 보호를 위한 GDPR 준수 동의 시스템

## 🎪 데모 및 스크린샷

### 메인 화면
- 실시간 동작 인식 (좌측)
- 줌바 가이드 영상 (우측)

### 분석 결과 화면
- 아이들을 위한 대형 점수 표시
- 부모님을 위한 상세 운동 리포트

## 🤝 기여하기

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참조하세요.

## 👥 개발팀

**Team GrooveToGrow**
- 해커톤 출품작
- 아동 건강과 기술의 융합을 추구하는 팀

## 📞 연락처

프로젝트에 대한 문의나 제안사항이 있으시면 GitHub Issues를 통해 연락해 주세요.

---

**Groove to Grow** - 아이들의 건강한 성장을 위한 AI 홈트레이닝 플랫폼 🌟