# LaserFlow Background Integration

이 저장소는 Three.js 기반 LaserFlow 셰이더 효과를 정적 페이지 배경에 적용한 예시입니다. 기존 포트폴리오 사이트에 이 효과를 반영하고 싶다면 아래 순서를 따르면 됩니다.

## 1. 파일 복사
- `laser-flow.js` 파일 전체를 프로젝트로 복사합니다.
- 페이지에 전역 스타일을 추가할 수 있도록, `index.html` `<style>` 블록에 포함된 `#laserflow-background` 관련 CSS 를 함께 복사합니다.

## 2. 마크업 추가
`<body>` 시작 부분에 다음 컨테이너를 추가합니다. 이 요소가 셰이더 캔버스를 붙잡는 고정 배경 레이어가 됩니다.

```html
<body>
  <div id="laserflow-background" aria-hidden="true"></div>
  <!-- 나머지 콘텐츠 -->
</body>
```

## 3. 스크립트 연결
페이지 맨 아래(보통 `</body>` 직전)에 LaserFlow 모듈을 로드합니다. 정적 호스팅이라면 상대 경로를 맞춰 주세요.

```html
<script type="module" src="./laser-flow.js"></script>
```

이 스크립트는 DOMContentLoaded 이벤트가 발생하면 자동으로 `#laserflow-background` 요소를 찾아 셰이더를 초기화합니다. Three.js 는 CDN(`https://unpkg.com/three`)에서 모듈 형태로 가져오므로 추가 설정이 필요하지 않습니다.

## 4. 커스터마이징
`laser-flow.js`에서 `mountLaserFlow` 호출 시 전달한 옵션을 조정하면 빔 색상, 위치, 안개 강도 등을 손쉽게 변경할 수 있습니다.

```js
const flow = mountLaserFlow(container, {
  color: '#FF79C6',
  horizontalBeamOffset: 0.08,
  verticalBeamOffset: -0.05,
  fogIntensity: 0.36,
  flowStrength: 0.18,
  wispIntensity: 4.4,
  flowSpeed: 0.38,
  mouseTiltStrength: 0.012,
});
```

필요 시 Three.js 를 직접 번들링 환경(예: React, Next.js)으로 옮길 수도 있으며, 이 경우 `import * as THREE from 'three';` 로 바꾼 뒤 프로젝트의 번들러가 해당 의존성을 처리하도록 설정하면 됩니다.

## 5. 퍼포먼스 참고
스크립트는 750ms 간격으로 FPS 를 샘플링하면서 기기 성능에 따라 DPR(픽셀 비율)을 자동으로 조절합니다. 성능이 낮은 기기에서는 자동으로 해상도를 낮춰 부드러움을 유지하고, 숨겨진 탭에서는 렌더링을 일시 중지하여 리소스를 절약합니다.

위 단계를 수행하면 기존 페이지에 동일한 LaserFlow 배경 효과를 적용할 수 있습니다.
