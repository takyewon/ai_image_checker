<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>AI 합성 이미지 의심 체크 웹툴</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    * {
      box-sizing: border-box;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }
    body {
      margin: 0;
      background: #0f172a;
      color: #e5e7eb;
      display: flex;
      justify-content: center;
      padding: 24px;
    }
    .container {
      width: 100%;
      max-width: 960px;
      background: rgba(15, 23, 42, 0.9);
      border-radius: 24px;
      padding: 24px 24px 32px;
      box-shadow: 0 24px 60px rgba(0, 0, 0, 0.6);
      border: 1px solid rgba(148, 163, 184, 0.3);
      backdrop-filter: blur(10px);
    }
    h1 {
      margin: 0 0 4px;
      font-size: 1.8rem;
      font-weight: 700;
    }
    .subtitle {
      margin-bottom: 16px;
      font-size: 0.95rem;
      color: #94a3b8;
    }
    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 4px 10px;
      border-radius: 999px;
      background: rgba(59, 130, 246, 0.1);
      color: #bfdbfe;
      font-size: 0.75rem;
      margin-bottom: 12px;
    }
    .badge span.icon {
      font-size: 0.9rem;
    }
    .grid {
      display: grid;
      grid-template-columns: minmax(0, 1.1fr) minmax(0, 1.3fr);
      gap: 20px;
    }
    @media (max-width: 780px) {
      .grid {
        grid-template-columns: minmax(0, 1fr);
      }
    }
    .card {
      background: radial-gradient(circle at top left, #1e293b, #020617);
      border-radius: 20px;
      padding: 16px 18px 18px;
      border: 1px solid rgba(148, 163, 184, 0.35);
    }
    .card-title {
      font-size: 0.95rem;
      font-weight: 600;
      margin-bottom: 8px;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .card-title span.icon {
      font-size: 1rem;
    }
    .card-desc {
      font-size: 0.8rem;
      color: #9ca3af;
      margin-bottom: 10px;
    }
    .upload-area {
      border-radius: 16px;
      border: 1px dashed rgba(148, 163, 184, 0.7);
      padding: 14px;
      text-align: center;
      background: rgba(15, 23, 42, 0.9);
      cursor: pointer;
      transition: background 0.15s, border-color 0.15s, transform 0.05s;
    }
    .upload-area:hover {
      border-color: #60a5fa;
      background: rgba(15, 23, 42, 1);
      transform: translateY(-1px);
    }
    .upload-area span.label {
      display: block;
      font-size: 0.82rem;
      color: #cbd5f5;
      margin-bottom: 4px;
    }
    .upload-area span.help {
      display: block;
      font-size: 0.73rem;
      color: #9ca3af;
    }
    .upload-area input[type="file"] {
      display: none;
    }
    .preview-wrapper {
      margin-top: 10px;
      max-height: 260px;
      border-radius: 14px;
      overflow: hidden;
      background: #020617;
      border: 1px solid rgba(30, 64, 175, 0.8);
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .preview-wrapper img {
      max-width: 100%;
      max-height: 260px;
      display: block;
    }
    .placeholder-text {
      font-size: 0.8rem;
      color: #64748b;
      padding: 30px 10px;
    }
    .checklist-group-title {
      font-size: 0.8rem;
      font-weight: 600;
      color: #cbd5f5;
      margin-top: 8px;
      margin-bottom: 4px;
      text-transform: uppercase;
      letter-spacing: 0.04em;
    }
    .checklist {
      display: grid;
      grid-template-columns: minmax(0, 1fr) minmax(0, 1fr);
      gap: 6px 14px;
      margin-top: 4px;
    }
    @media (max-width: 780px) {
      .checklist {
        grid-template-columns: minmax(0, 1fr);
      }
    }
    label.checkbox {
      display: flex;
      align-items: flex-start;
      gap: 6px;
      font-size: 0.78rem;
      cursor: pointer;
      color: #e5e7eb;
    }
    label.checkbox input {
      margin-top: 2px;
    }
    .hint {
      font-size: 0.75rem;
      color: #9ca3af;
      margin-top: 4px;
    }
    .button-row {
      margin-top: 12px;
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      align-items: center;
      justify-content: space-between;
    }
    .primary-btn {
      border: none;
      border-radius: 999px;
      padding: 9px 16px;
      font-size: 0.78rem;
      font-weight: 600;
      background: linear-gradient(135deg, #2563eb, #7c3aed);
      color: #e5e7eb;
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      box-shadow: 0 10px 25px rgba(37, 99, 235, 0.4);
      transition: transform 0.08s, box-shadow 0.08s, filter 0.08s;
    }
    .primary-btn:hover {
      transform: translateY(-1px);
      filter: brightness(1.05);
      box-shadow: 0 12px 28px rgba(37, 99, 235, 0.6);
    }
    .primary-btn span.icon {
      font-size: 0.9rem;
    }
    .secondary-btn {
      border-radius: 999px;
      padding: 7px 12px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      background: transparent;
      color: #9ca3af;
      font-size: 0.72rem;
      cursor: pointer;
    }
    .secondary-btn:hover {
      border-color: #e5e7eb;
      color: #e5e7eb;
    }
    .result-card {
      margin-top: 12px;
      border-radius: 16px;
      padding: 10px 12px;
      background: radial-gradient(circle at top right, rgba(59, 130, 246, 0.35), #020617);
      border: 1px solid rgba(129, 140, 248, 0.7);
    }
    .result-title {
      font-size: 0.85rem;
      font-weight: 600;
      margin-bottom: 4px;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .result-pill {
      display: inline-flex;
      align-items: center;
      padding: 3px 8px;
      border-radius: 999px;
      font-size: 0.7rem;
      font-weight: 600;
      margin-left: auto;
    }
    .result-pill.low {
      background: rgba(22, 163, 74, 0.17);
      color: #bbf7d0;
      border: 1px solid rgba(34, 197, 94, 0.7);
    }
    .result-pill.medium {
      background: rgba(245, 158, 11, 0.17);
      color: #fed7aa;
      border: 1px solid rgba(251, 191, 36, 0.7);
    }
    .result-pill.high {
      background: rgba(220, 38, 38, 0.2);
      color: #fecaca;
      border: 1px solid rgba(248, 113, 113, 0.7);
    }
    .result-text {
      font-size: 0.78rem;
      color: #e5e7eb;
      margin-bottom: 3px;
    }
    .result-subtext {
      font-size: 0.73rem;
      color: #9ca3af;
    }
    .disclaimer {
      margin-top: 16px;
      font-size: 0.7rem;
      color: #9ca3af;
      padding: 10px 12px;
      border-radius: 12px;
      background: rgba(15, 23, 42, 0.9);
      border: 1px dashed rgba(148, 163, 184, 0.6);
    }
    .disclaimer strong {
      color: #e5e7eb;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="badge">
      <span class="icon">🧪</span>
      <span>실험적 교육용 도구 · 실제 AI 판별기가 아닙니다</span>
    </div>
    <h1>AI 합성 이미지 의심 체크 웹툴</h1>
    <div class="subtitle">
      이미지를 올린 뒤 아래 체크리스트를 기반으로<br />
      <strong>“이 이미지가 AI 합성일 가능성을 스스로 점검”</strong>해보는 대학생용 도구입니다.
    </div>

    <div class="grid">
      <!-- 왼쪽: 이미지 업로드 / 미리보기 -->
      <div class="card">
        <div class="card-title">
          <span class="icon">🖼️</span>
          <span>1. 의심되는 이미지 불러오기</span>
        </div>
        <div class="card-desc">
          분석하려는 사진 또는 스크린샷을 불러와 주세요. (JPG, PNG 등)
        </div>

        <label class="upload-area">
          <input type="file" id="imageInput" accept="image/*" />
          <span class="label">이미지 클릭 또는 드래그하여 업로드</span>
          <span class="help">딥페이크로 의심되는 사진, 이상해 보이는 기록 사진 등을 올려 보세요.</span>
        </label>

        <div class="preview-wrapper" id="previewWrapper">
          <div class="placeholder-text">
            아직 이미지가 없습니다.<br />
            위 영역을 클릭해서 이미지를 선택하면 이곳에 미리보기가 표시됩니다.
          </div>
        </div>
      </div>

      <!-- 오른쪽: 체크리스트 + 결과 -->
      <div class="card">
        <div class="card-title">
          <span class="icon">🔍</span>
          <span>2. 시각적 특징 체크리스트</span>
        </div>
        <div class="card-desc">
          아래 항목 중 해당된다고 느껴지는 부분을 체크해 주세요.<br />
          체크 수가 많을수록 AI 합성 가능성이 높을 수 있습니다.
        </div>

        <div class="checklist-group-title">얼굴 / 눈 / 손</div>
        <div class="checklist">
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            눈동자 방향이 서로 다르게 보이거나, 시선이 애매하게 틀어져 있다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            손가락 개수가 이상하거나, 손 모양이 부자연스럽다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            귀, 치아, 안경 다리 등 세부 구조가 이상하게 뭉개져 있다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            얼굴 윤곽(턱, 광대 등)이 사진마다 미묘하게 달라 보인다.
          </label>
        </div>

        <div class="checklist-group-title">질감 / 배경 / 조명</div>
        <div class="checklist">
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            피부 질감이 너무 매끈하거나, 브러시로 칠한 것처럼 균일하다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            배경 사람이 흐릿하거나, 머리카락과 배경이 이상하게 섞여 있다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            그림자 방향이나 밝기·조명이 주변 사물과 일관되지 않는다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            귀걸이·목걸이·문양 같은 반복 패턴이 어딘가 깨져 있다.
          </label>
        </div>

        <div class="checklist-group-title">텍스트 / 맥락 / 메타정보</div>
        <div class="checklist">
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            사진 속 간판·책·로고의 글자가 비틀리거나 읽기 어렵다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            역사적 사건·공식 행사치고는 출처가 애매하거나 뉴스를 찾기 힘들다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            원본 촬영자, 언론사, 기관 정보가 명확히 제시되어 있지 않다.
          </label>
          <label class="checkbox">
            <input type="checkbox" class="cl-item" value="1" />
            너무 극적이거나 자극적인 장면인데, 다른 각도/출처의 사진이 없다.
          </label>
        </div>

        <div class="hint">
          ※ 이 도구는 실제 AI 모델이 아닌, <strong>AI 판별 원리를 반영한 체크리스트 기반</strong> 도구입니다.
        </div>

        <div class="button-row">
          <button class="primary-btn" id="analyzeBtn">
            <span class="icon">⚡</span>
            <span>분석하기</span>
          </button>
          <button class="secondary-btn" id="resetBtn">체크 초기화</button>
        </div>

        <div class="result-card" id="resultCard" style="display: none;">
          <div class="result-title">
            <span>분석 결과</span>
            <span id="resultPill" class="result-pill">-</span>
          </div>
          <div class="result-text" id="resultText"></div>
          <div class="result-subtext" id="resultSubtext"></div>
        </div>
      </div>
    </div>

    <div class="disclaimer">
      <strong>중요 안내</strong><br />
      이 웹툴은 연구·교육용으로 설계된 <strong>간단한 보조 도구</strong>이며,
      실제 법적·기술적 의미의 AI 판별기가 아닙니다.<br />
      체크 항목 수가 많다고 해서 반드시 AI 합성 이미지라는 뜻은 아니며,
      반대로 체크가 적다고 해서 반드시 안전하다는 보장도 없습니다.<br />
      중요한 사안(역사 자료, 범죄 증거, 언론 사진 등)의 경우
      <strong>공식 기관·전문가의 검증</strong>과 신뢰할 만한 출처를 반드시 함께 확인해야 합니다.
    </div>
  </div>

  <script>
    // 이미지 업로드 및 미리보기
    const imageInput = document.getElementById("imageInput");
    const previewWrapper = document.getElementById("previewWrapper");

    imageInput.addEventListener("change", function (event) {
      const file = event.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function (e) {
        previewWrapper.innerHTML = "";
        const img = document.createElement("img");
        img.src = e.target.result;
        previewWrapper.appendChild(img);
      };
      reader.readAsDataURL(file);
    });

    // 체크리스트 분석
    const analyzeBtn = document.getElementById("analyzeBtn");
    const resetBtn = document.getElementById("resetBtn");
    const resultCard = document.getElementById("resultCard");
    const resultPill = document.getElementById("resultPill");
    const resultText = document.getElementById("resultText");
    const resultSubtext = document.getElementById("resultSubtext");

    function analyzeChecklist() {
      const items = document.querySelectorAll(".cl-item");
      let score = 0;
      items.forEach((item) => {
        if (item.checked) {
          score += Number(item.value || 1);
        }
      });

      let level = "";
      let pillClass = "";
      let mainText = "";
      let subText = "";

      if (score <= 3) {
        level = "의심 낮음";
        pillClass = "low";
        mainText =
          "현재 체크된 항목이 많지 않아, 시각적 단서만으로는 강한 AI 합성 의심은 어렵습니다.";
        subText =
          "다만, 여전히 출처(언론사·촬영자·기관)와 맥락(언제, 어디서, 누가 촬영했는지)을 함께 확인하는 것이 안전합니다.";
      } else if (score <= 7) {
        level = "의심 보통";
        pillClass = "medium";
        mainText =
          "여러 지점에서 인공적인 느낌이 감지됩니다. AI 합성일 가능성을 염두에 두고 추가 검증이 필요해 보입니다.";
        subText =
          "다른 매체의 보도 여부, 동일 사건을 다룬 사진·영상, 팩트체크 기사 등을 통해 교차 검증해 보세요.";
      } else {
        level = "의심 높음";
        pillClass = "high";
        mainText =
          "시각적으로 부자연스러운 요소가 많이 관찰됩니다. AI 합성 이미지일 가능성이 상당히 높습니다.";
        subText =
          "이 이미지를 그대로 신뢰하기보다는, 공식 출처를 찾거나 전문가·기관의 검증을 거친 뒤 활용하는 것이 좋습니다.";
      }

      resultPill.textContent = level;
      resultPill.className = "result-pill " + pillClass;
      resultText.textContent = `체크된 항목 수: ${score}개 · 종합 판단: ${level}`;
      resultSubtext.textContent = subText;
      resultCard.style.display = "block";
    }

    analyzeBtn.addEventListener("click", analyzeChecklist);

    resetBtn.addEventListener("click", () => {
      const items = document.querySelectorAll(".cl-item");
      items.forEach((item) => (item.checked = false));
      resultCard.style.display = "none";
    });
  </script>
</body>
</html>
