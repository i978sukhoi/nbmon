# nbmon

**크로스플랫폼 네트워크 대역폭 모니터**

Rust로 작성된 빠르고 크로스플랫폼 네트워크 대역폭 모니터링 도구로, Linux의 `nload`와 `bmon`에서 영감을 받았습니다.

`nbmon`은 성능과 사용성에 중점을 두고 터미널에서 실시간 네트워크 트래픽 시각화 및 통계를 제공합니다. Windows와 Linux 사용자 모두에게 익숙한 Linux 네트워크 모니터링 도구의 경험을 제공합니다.

## ✨ 주요 기능

- **🚀 실시간 네트워크 대역폭 모니터링** - 라이브 차트 제공
- **📊 다양한 표시 모드**: 향상된 TUI, 클래식 TUI, 단순 콘솔 출력
- **🖥️ 크로스플랫폼 지원**: Windows와 Linux
- **⚡ 높은 성능**: 병렬 통계 수집으로 44% 성능 향상  
- **🎯 인터페이스 선택**: 키보드 단축키로 네트워크 인터페이스 간 탐색
- **📈 기록 데이터**: 60초 대역폭 기록과 스파크라인 그래프
- **🛠️ 강력한 에러 처리**: 포괄적인 에러 보고와 우아한 폴백
- **🔍 성능 벤치마킹**: 수집 효율성 측정을 위한 내장 도구

## 🚀 빠른 시작

### 빌드 요구사항

#### Rust 설치
```bash
# Rust가 설치되어 있지 않은 경우
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

#### Linux - musl 도구 설치 (정적 링크 빌드용)

**Ubuntu/Debian:**
```bash
sudo apt-get update
sudo apt-get install musl-tools
```

**Fedora/RHEL:**
```bash
sudo dnf install musl-gcc musl-devel
```

**Arch Linux:**
```bash
sudo pacman -S musl
```

#### Windows
- [Rust 공식 설치 프로그램](https://rustup.rs/) 사용
- Visual Studio C++ Build Tools 또는 MinGW 필요

### 설치 및 빌드

```bash
git clone https://github.com/username/nbmon.git
cd nbmon

# musl target 추가 (Linux - 정적 링크용)
rustup target add x86_64-unknown-linux-musl

# 릴리즈 빌드
cargo build --release

# 빌드된 바이너리 위치
# Linux (musl): ./target/x86_64-unknown-linux-musl/release/nbmon
# Windows: ./target/release/nbmon.exe
```

> **참고**: 이 프로젝트는 정적 링크 빌드를 기본으로 사용하여 GLIBC 버전 호환성 문제를 해결합니다. 
> 빌드된 바이너리는 대부분의 Linux 배포판에서 추가 의존성 없이 실행됩니다.

### 사용법

```bash
# 기본 향상된 TUI 모드
./target/x86_64-unknown-linux-musl/release/nbmon

# 클래식 TUI 모드  
./target/x86_64-unknown-linux-musl/release/nbmon --classic

# 단순 콘솔 출력
./target/x86_64-unknown-linux-musl/release/nbmon --simple

# 성능 벤치마크
cargo run --example benchmark_parallel
```

## 🎮 조작법

### 향상된 TUI 모드
- **←/→ 또는 h/l**: 네트워크 인터페이스 전환
- **Space**: 통계 수동 새로고침
- **r**: 대역폭 기록 및 최대 속도 초기화
- **q**: 애플리케이션 종료

### 클래식 TUI 모드
- **↑/↓**: 인터페이스 목록 탐색
- **q**: 애플리케이션 종료

## 📋 시스템 요구사항

- **Windows**: 네트워크 접근을 위한 관리자 권한이 있는 Windows 10/11
- **Linux**: `/proc/net/dev` 및 `/sys/class/net` 지원이 있는 최신 배포판
- **CPU**: 최적의 병렬 성능을 위해 멀티코어 권장
- **메모리**: 최소 (일반적으로 < 10MB 사용)

## 🏗️ 아키텍처

```
nbmon/
├── src/
│   ├── main.rs              # 애플리케이션 진입점
│   ├── lib.rs               # 라이브러리 루트 및 내보내기
│   ├── error.rs             # 에러 처리 및 디버깅
│   ├── network/             # 네트워크 모니터링 레이어
│   │   ├── interface.rs     # 네트워크 인터페이스 관리
│   │   ├── stats.rs         # 통계 수집 및 계산
│   │   ├── parallel_stats.rs # 고성능 병렬 수집
│   │   ├── windows_api.rs   # Windows 전용 네트워크 API
│   │   └── linux_api.rs     # Linux 전용 네트워크 API  
│   ├── ui/                  # 사용자 인터페이스 레이어
│   │   ├── app.rs           # 클래식 TUI 애플리케이션
│   │   ├── app_improved.rs  # 차트가 있는 향상된 TUI
│   │   └── widgets/         # 커스텀 UI 컴포넌트
│   └── utils/               # 유틸리티 함수
└── examples/                # 사용 예제 및 벤치마크
```

## 🔧 개발

### 빌드 명령어
- `cargo build` - 디버그 빌드
- `cargo build --release` - 최적화된 릴리즈 빌드
- `cargo run` - 향상된 TUI 모드 실행
- `cargo run -- --classic` - 클래식 TUI 모드 실행
- `cargo run -- --simple` - 단순 콘솔 모드 실행
- `cargo run --example benchmark_parallel` - 성능 벤치마크
- `cargo clean` - 빌드 산출물 정리

### 테스트 및 품질
- `cargo test` - 모든 테스트 실행
- `cargo check` - 컴파일 에러 확인
- `cargo clippy` - 코드 품질 린트
- `cargo fmt` - 코드 포맷팅

## 📊 성능

NBMon은 최적의 성능을 위해 병렬 통계 수집을 사용합니다:

- 멀티코어 시스템에서 순차 수집보다 **44% 빠름**
- 활성 인터페이스 모니터링에서 **2.5배 속도 향상**
- CPU 코어 수에 따른 자동 스케일링
- 필요시 순차 처리로 우아한 폴백

## 🤝 기여하기

기여를 환영합니다! 이슈, 기능 요청 또는 풀 리퀘스트를 자유롭게 제출해 주세요.

## 📄 라이선스

이 프로젝트는 MIT 또는 Apache-2.0 라이선스로 배포됩니다.

## 🙏 감사의 글

- Linux `nload` 및 `bmon` 도구에서 영감을 받았습니다
- [Rust](https://rust-lang.org/), [Ratatui](https://github.com/ratatui/ratatui), [Rayon](https://github.com/rayon-rs/rayon)으로 구축되었습니다
- Windows API 및 Linux `/proc` 파일시스템으로 구동되는 크로스플랫폼 네트워킹
