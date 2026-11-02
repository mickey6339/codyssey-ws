# Codyssey Workstation Setup & 실습 보고서

본 저장소는 개발 환경(VS Code, Git, Linux 기본 권한, Docker) 구축 및 실습 과제 수행 증거와 기술 정리를 포함한 공식 제출물입니다.

---

## 1. 디렉토리 구조 및 구성 기준 (평가 항목 #10)

```text
codyssey-ws/
├── index.html       # 웹 서비스 실습용 기본 HTML 파일
├── practice/        # 터미널 및 파일 권한/관리 실습용 디렉토리
│   ├── empty.txt    # 빈 파일 생성 및 경로 실습용
│   └── test.txt     # 텍스트 수정 및 Git 변경 추적 실습용
├── Dockerfile       # Nginx 기반 정적 웹 서빙 컨테이너 빌드 명세서
└── README.md        # 전체 환경 구성, 실행 로그, 기술 개념 및 트러블슈팅 보고서