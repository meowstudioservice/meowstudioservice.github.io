# Production 기준 출시노트

## 기준 확정

- 조회 대상은 해당 packageName의 실제 Production 출시 이력이다. 비공개/내부/공개 테스트, draft, 검토 중, 승인됐지만 게시되지 않은 릴리즈는 기준이 아니다.
- 단계적 Production 배포라도 실제 사용자에게 배포가 시작됐다면 출시된 버전이다. 여러 활성 릴리즈, 중단·롤백, 기기별 버전이 있으면 Console 출시 이력에서 마지막 실제 출시를 확인한다. API `edits.tracks.get`의 활성 릴리즈 목록이나 가장 큰 versionCode만으로 시간 순서·실제 게시 여부를 확정하지 않는다. 모호하면 그 부분을 확인하고, 직전 커밋으로 대체하지 않는다.
- API의 `completed` 또는 `inProgress`는 트랙 설정 상태이며 조회 중인 edit에 아직 게시되지 않은 변경이 있을 수 있다. 실제 게시 여부는 Console 게시 개요/출시 이력 등으로 교차 확인한다.
- Production 출시가 정말 한 번도 없으면 최초 출시로 기록하고, 이번 앱에 실제 포함된 사용자 기능으로 최초 출시노트를 작성한다. 인증 오류·빈 응답·매핑 실패를 최초 출시로 처리하지 않는다.

## Git 매핑과 변경 범위

1. Production `versionName`과 `versionCode`를 확인하고 원격 태그 `v<versionName>+<versionCode>`의 커밋을 찾는다. 태그의 버전 파일이 Play 값과 일치하는지 검증한다.
2. 태그가 없으면 당시 빌드·배포 기록에서 검증 가능한 소스 SHA를 찾는다. 추정 태그를 생성하지 않는다. 매핑할 수 없으면 기준이 미확인이라고 보고하고 확정 전 출시노트 제출을 중단한다.
3. 후보가 기준 커밋을 포함하는지 `git merge-base --is-ancestor <production-sha> <candidate-sha>`로 확인한다. 일반 경우 `git log <production-sha>..<candidate-sha>`와 `git diff <production-sha> <candidate-sha>` 및 관련 PR을 함께 검토한다. 조상 관계가 없으면 롤백·분기 이력을 조사하고 실제 사용자 변경을 검토한다.
4. 예: Production `1.0.0+20`, 비공개 테스트 `1.0.1+21`, 새 후보 `1.0.2+22`이면 **20 → 22** 누적 변경을 쓴다. **21 → 22**만 쓰지 않는다. Production이 21로 승격된 뒤에는 21이 기준이다.
5. 사용자용 출시노트는 실제 추가·수정·개선 위주로 정리한다. 내부 분석 기록에는 기준 SHA와 후보 SHA, 조회 시각, 근거 링크, 검증 결과를 남긴다. API 토큰·서명 비밀값은 저장하지 않는다.

## 공식 참고

- [Play 릴리즈 준비 및 출시](https://support.google.com/googleplay/android-developer/answer/9859348)
- [게시 상태](https://support.google.com/googleplay/android-developer/answer/9859751)
- [트랙 리소스와 상태](https://developers.google.com/android-publisher/api-ref/rest/v3/edits.tracks)

실행 시 도구/API의 최신 동작을 확인한다. 이 참고 문서는 인증 정보나 실제 앱의 Production 버전을 저장하는 곳이 아니다.
