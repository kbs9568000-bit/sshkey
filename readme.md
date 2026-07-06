# 📊 Google Apps Script 기반 HTML 웹 폼 데이터 수집 프로젝트

Google Apps Script(GAS)를 활용하여 웹 페이지의 HTML `<form>` 태그로부터 입력된 데이터를 구글 스프레드시트(Google Sheets)로 실시간 전송하고 저장하는 토이 프로젝트입니다. 별도의 백엔드 서버 없이 구글 인프라를 활용하여 서버리스(Serverless) 데이터 수집 파이프라인을 구축할 수 있습니다.

---

## 🛠 기술 스택 (Tech Stack)

* **Frontend:** HTML5, CSS3, JavaScript (Fetch API / Axios)
* **Backend:** Google Apps Script (V8 Runtime)
* **Database:** Google Sheets

---

## 🚀 주요 기능 (Features)

* **서버리스 데이터 수집:** 웹 페이지 내 `<form>` 요소를 통해 제출된 데이터를 구글 시트에 실시간 적재
* **CORS 이슈 해결:** `HtmlService` 또는 `ContentService`를 활용하여 외부 도메인에서의 비동기 API 요청(`POST`) 처리
* **구글 API 확장성:** 데이터 저장과 동시에 구글 설문지 연동, 지메일(Gmail) 자동 알림 등 확장 가능

---

## 💻 설치 및 사용 방법 (Setup Instructions)

### 1. 구글 스프레드시트 및 스크립트 설정
1. 새 **구글 스프레드시트**를 생성합니다.
2. 상단 메뉴에서 **확장 프로그램** ➡️ **Apps Script**를 클릭합니다.
3. 기존 코드를 지우고 아래의 `Code.gs` 스크립트를 붙여넣습니다.

```javascript
// Code.gs
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var params = e.parameter;
    
    // 스프레드시트에 기록할 데이터 배열 매핑 (HTML form의 name 속성과 일치해야 함)
    var rowData = [
      new Date(), // 제출 시간
      params.name,
      params.email,
      params.message
    ];
    
    sheet.appendRow(rowData);
    
    return ContentService.createTextOutput(JSON.stringify({"result": "success"}))
                         .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({"result": "error", "error": error.toString()}))
                         .setMimeType(ContentService.MimeType.JSON);
  }
}
'''