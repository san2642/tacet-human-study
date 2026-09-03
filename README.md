# TACET Human Study

GitHub Pages용 단일 파일 설문 페이지입니다. `index.html`과 `clips/` 폴더의 영상을 저장소에 올리면 됩니다.

## 시작 전 설정

1. Firebase Console에서 새 프로젝트를 만들고 **Authentication > Sign-in method > Anonymous**를 활성화합니다.
2. **Firestore Database**를 만든 뒤 아래 규칙을 적용합니다. 참가자는 응답을 쓸 수 있지만 읽을 수 없습니다.

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /responses/{responseId} {
      allow create: if request.auth != null;
      allow read, update, delete: if false;
    }
  }
}
```

3. Firebase의 웹 앱 설정값을 `index.html`의 `firebaseConfig`에 붙여 넣습니다.
4. `scenarios`의 `dbA` 배열을 각 시나리오에서 방법 순서 `Ours, VLM-Social-Nav, MUTE, RPP, SFM`에 맞는 실제 측정값으로 바꿉니다.
5. `clips/` 폴더에 `focused-work-1.mp4`부터 `pedestrian-5.mp4`까지 20개 영상을 넣습니다. 각 번호는 위 방법 순서에 대응합니다.
6. GitHub 저장소 **Settings > Pages**에서 `main` 또는 `master` 브랜치의 루트 폴더를 배포 대상으로 선택합니다.

## 저장되는 필드

각 응답은 Firestore의 `responses` 컬렉션에 영상별 한 건으로 저장됩니다. `participant_id`, 무작위 제시 순서, 영상 ID, 시나리오, 방법, dBA, Q1–Q4, 제출 시각이 포함됩니다.

Firestore Console에서 컬렉션을 CSV로 내보내거나, 분석 스크립트에서 Firestore Admin SDK를 사용해 내려받을 수 있습니다.
