---
title: Electron
last_modified_at: null
categories:
  - Post Formats
tags:
  - Post Formats
  - readability
  - standard
date: 2024-04-21T09:09:13.168Z
draft: false
---

<br>
# 목차
1. 데스크톱 어플리케이션
2. Electron
3. Electron 구조
4. 스크립트 사용  

---  
<br>

# 데스크톱 애플리케이션

![desktop](/assets/desktopapp.webp)

- **네이티브 애플리케이션**   
특정 OS에 최적화되어 개발된 프로그램으로, 해당 OS의 API와 기능을 직접적으로 사용할 수 있는 애플리케이션. Windows에서는 .NET Framework,  macOS는 Swift 등을 사용한다. (e.g. Chrome, Microsoft Word, Adobe Photoshop)
- **웹 애플리케이션**   
웹 브라우저를 통해 사용되는 프로그램으로 OS나 디바이스에 의존하진 않지만, 실행되는 브라우저 기능에 의해 제한된다. 구글 크롬, 모질라 파이어폭스, 애플 사파리, 윈도우 브라우저 등 각 브라우저에서 허용하는 범위에서만 작동 가능하다. (e.g. Google Docs, Discord web)
- **프로그레시브 웹 앱(Progressive Web Apps, PWA)**   
기본적으로 모바일 또는 데스크톱 앱처럼 보이고 작동할 수 있도록 몇 줄의 코드가 추가된 웹 앱으로, 데스크톱과 모바일 기기 모두에 설치해서 사용할 수 있다. 하지만 PWA는 브라우저를 통해 실행되므로, 네이티브 앱이 제공하는 OS 수준의 기능과 API에 완전하게 접근할 수 없다. 예를 들어, 블루투스, NFC, 고급 카메라 컨트롤, 특정 백그라운드 작업 등의 기능이 제한적일 수 있다.(e.g. 파파고, 구글 캘린더)
- **데스크톱 컨테이너 (Electron)**   
자바스크립트, HTML, CSS와 같은 웹 기술을 사용하여 데스크톱 앱을 구현할 수 있는 프레임워크.  웹 기술을 사용해서 작성할 수 있어서 데스크톱 앱과 웹 앱을 모두 쉽게 유지 관리할 수 있고, 단일 코드베이스를 공유할 수 있다는 장점이 있다. 특히 PWA에 비해 데스크톱 컨테이너의 큰 장점은 알림, 파일 시스템 접근, 프로세스 관리, 네트워크 요청 및 다중 창 관리와 같은 OS 기능에 액세스할 수 있다는 것이다. 
- **크로스 플랫폼(cross-platform)**   
소프트웨어를 개발할 때 단일 코드베이스를 사용하여 여러 OS  또는 디바이스에서 실행할 수 있도록 하는 접근 방식. 개발의 효율성과 편의성을 증진시키지만, 때로는 각 플랫폼의 고유한 기능이나 최적화를 100% 활용하지 못할 수도 있다. 플랫폼별로 UI/UX를 조정해야 할 필요가 있으며, 때로는 성능 저하가 발생할 수도 있다. (e.g. 웹앱, Qt, React Native, Flutter)

<br>
# Electron
GitHub에서 개발한 오픈 소스 라이브러리로, Node.js의 백엔드 기능과 Chromium의 웹 기술을 결합하여 웹 기술을 사용해 데스크톱 애플리케이션을 개발할 수 있게 해주는 프레임워크다. 
<br>(e.g. Discord, Figma, Slack, Spotify, VS Code)
- 장점
    - 웹기술 활용
    - 크로스플랫폼 : OS에 맞는 네이티브 기능을 제공하는 라이브러리와 모듈을 포함하고 있어, 추가적인 플랫폼별 코드 작성 없이도 이러한 기능을 애플리케이션에 통합할 수 있음
    - OS API 활용
    - 웹 버전을 데스크톱 버전과 거의 동일하게 만드는 것이 용이 (Discord 웹 버전이 대표적인 예시)
- 단점
    - 높은 자원 사용량: Electron이 기반하고 있는 Chromium의 단점과 상통하는 부분인데, 일반적으로 Electron으로 만들어진 앱들은 각자 다른 버전의 Electron을 사용하기 때문에 각각의 앱들이 Chromium 엔진을 따로 구동하는 형태로 구성되는 경우가 많고 이는 더더욱 높은 메모리 사용량으로 이어진다. 그 외에 백엔드 구동에 사용되는 Node.js 런타임의 메모리 사용량도 무시할 수 없는 수준이다.
    - 보안 : Node.js를 통해 웹 기술로 데스크톱 애플리케이션을 만들 수 있는 것뿐만 아니라 더 큰 권한을 갖게 된다. Electron에서는 js를 통해 파일시스템, 사용자 쉘 등에 접근이 가능하기 때문에 신뢰할 수 없는 임의의 콘텐츠를 표시하면 node.js 모듈 자체가 안전하지 않을 수 있다. 예를 들어 로컬 콘텐츠가 아닌, 온라인 소스코드를 실행하고 파일 경로와 관련된 API를 사용하여 악의적으로 파일 시스템에 접근해 제어하는 상황이 발생할 수도 있다.

<br>
# Electron 구조
![structure](/assets/structure.jpg)

## Main Process

Electron 애플리케이션의 진입점이자 Backend의 영역으로, Node.js에서 사용되는 모든 모듈들을 사용 할 수 있으며, 전체 애플리케이션의 생명주기를 관리한다. Electron 애플리케이션은 항상 하나의 메인 프로세스를 가지고 있으며, 렌더러 프로세스는 여러 개가 존재 할 수 있지만 메인 프로세스는 애플리케이션 당 항상 하나만 존재 할 수 있다.

- 애플리케이션 윈도우 생성 및 관리
- 시스템 메뉴, 대화상자, 다른 네이티브 요소와의 상호작용
- 렌더러 프로세스와의 통신
- 전체 애플리케이션의 생명주기 관리

```jsx
// main.js
const { app, BrowserWindow, ipcMain } = require('electron');

// 애플리케이션 윈도우 생성 함수
function createWindow () {
  // 새로운 브라우저 윈도우 생성
  let mainWindow= new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false // 보안상의 이유로 실제 애플리케이션에서는 true 권장
    }
  });

  // 윈도우에 로드할 HTML 파일
  mainWindow.loadFile('index.html');

  // 렌더러 프로세스에서 'ipc-message' 이벤트를 수신하고 응답
  ipcMain.on('ipc-message', (event, arg) => {
    console.log(arg);  // 예: "Hello from Renderer"
    event.reply('ipc-reply', 'Hi, Renderer. This is Main Process.');
  });
}

// 앱이 준비되었을 때 윈도우 생성
app.whenReady().then(createWindow);

```

**Main Process 와 Renderer Process의 차이점**

메인 프로세스는 BrowserWindow 클래스를 생성하여 웹 페이지를 만든다. Electron에서의 Window는 이 BrowserWindow클래스로 만들어 지며 이 BrowserWindow는 메인 프로세스에서만 사용 할 수 있습니다

- nodeIntegration
    - **목적**: **`nodeIntegration`** 옵션은 Electron의 렌더러 프로세스에서 Node.js 기능의 사용 여부를 결정합니다. 기본값은 **`false`**입니다.
    
    - **설정이 `true`일 때**: 렌더러 프로세스에서 Node.js의 모든 API를 사용할 수 있습니다. 이는 개발자가 Node.js의 파일 시스템 접근, 네트워크 기능 등을 사용할 수 있도록 하지만, 웹 콘텐츠(예: 원격 콘텐츠나 불신하는 소스의 스크립트)와 Node.js 코드가 같은 컨텍스트에서 실행되므로 보안 리스크를 증가시킬 수 있습니다.
    - **보안 취약점**: 악의적인 웹 콘텐츠가 Node.js의 기능을 사용하여 시스템을 손상시킬 수 있는 행동을 취할 가능성이 있습니다. 예를 들어, 파일 시스템을 조작하거나 기타 시스템 리소스에 악의적인 접근을 시도할 수 있습니다.
- contextIsolation
    - **목적**: **`contextIsolation`**은 렌더러 프로세스에서 웹 콘텐츠와 Electron 애플리케이션의 코드를 별도의 JavaScript 컨텍스트에서 실행하는 것을 말합니다. 기본값은 **`true`**입니다.
    - **설정이 `true`일 때**: 렌더러 프로세스의 전역 변수와 웹 페이지의 전역 변수가 분리되어, 웹 콘텐츠가 애플리케이션의 전역 스코프에 접근하는 것을 방지합니다. 이는 보안을 강화하는 데 중요한 역할을 하며, 웹 페이지가 애플리케이션의 전역 상태를 변경하거나 Node.js 환경을 악용하는 것을 방지합니다.
    - **보안 강화**: 컨텍스트 격리는 크로스-사이트 스크립팅(XSS) 공격으로부터 애플리케이션을 보호하는 데 도움을 줍니다. 악의적인 스크립트가 애플리케이션의 민감한 정보에 접근하거나 변경하는 것을 방지합니다.

## Renderer Process

렌더러 프로세스는 웹 페이지와 같으며, HTML, CSS, JavaScript로 이루어지는 frontend 영역이다. Electron은 웹 페이지를 보여주기 위해 Chromium을 사용하고 있기 때문에 Chromium의 멀티 프로세스 아키텍처가 그대로 이용되고 있다. Electron 안에서 보여지는 각각의 웹 페이지는 렌더러 프로세스 안에서 동작한다.

```jsx
//renderer.js 

const { ipcRenderer } = require('electron');

// 버튼 클릭 이벤트 리스너 등록
document.getElementById('messageBtn').addEventListener('click', () => {
  // 메인 프로세스에 메시지 전송
  ipcRenderer.send('ipc-message', 'Hello from Renderer');
});

// 메인 프로세스로부터의 응답 수신
ipcRenderer.on('ipc-reply', (event, arg) => {
  console.log(arg); // 예: "Hi, Renderer. This is Main Process."
  alert(arg);
});

```

### IPC (Inter-Process Communication)

메인 프로세스는 렌더러 프로세스와 IPC를 사용하여 비동기적 또는 동기적으로 통신할 수 있다. 이를 통해 렌더러 프로세스들 간에 데이터를 전송하거나 이벤트를 발생시킬 수 있다. 위 코드 예시에서 ipc 통신은 다음 순서로 이뤄진다. (렌더러에서 네이티브 GUI관련 API 호출은 허용하면 안된다. 왜냐하면 이것은 매우 위험한 일이고, 리소스 릭을 발생시키기 쉽기 때문. 렌더러에서 GUI 작업을 수행하려면 메인 프로세스에게 네이티브GUI 관련 작업을 수행할 수 있도록 요청해야 한다.)

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
<head>
  <title>Electron Example</title>
</head>
<body>
  <h1>Communication between Main and Renderer Processes</h1>
  <button id="messageBtn">Send Message to Main</button>
  <script src="renderer.js"></script>
</body>
</html>
```

1. **렌더러 프로세스**는 버튼 클릭 시 메인 프로세스에 'ipc-message' 이벤트를 보낸다.
2. **메인 프로세스**는 이 메시지를 수신하고, 'ipc-reply' 이벤트로 응답을 보낸다.
3. **렌더러 프로세스**는 메인 프로세스의 응답을 받고 사용자에게 알린다.

## 스크립트를 통한 OS API 접근

```jsx
// main.js
const scriptPath = getAssetPath(path.join('python_modules', 'tracker.py'));
const pyPath = getAssetPath(path.join('python_modules', 'venv', 'Scripts', 'python.exe'));
const pyshell = new PythonShell(scriptPath, { pythonPath: pyPath })

function createWindow () {
	...
  pyshell.on('message', function (message) {
    console.log('Received message from Python script:', message);
  });
	...
}
```

```python
# python_module.py
import sys

print("######### python script is running #########")
sys.stdout.flush() 
```
