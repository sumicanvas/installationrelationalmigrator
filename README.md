# MongoDB Relational Migrator macOS 설치 가이드

작성 기준일: 2026-08-21

## 설치 가능 여부

설치할 수 있습니다. 이 Mac은 MongoDB Relational Migrator의 공식 요구 사항을 충족합니다.

| 항목 | 이 Mac | 공식 요구 사항 | 결과 |
| --- | --- | --- | --- |
| macOS | 26.6.2 | macOS 11 이상 | 충족 |
| CPU | Apple Silicon (`arm64`) | Intel 또는 ARM | 충족 |
| 메모리 | 24GB | 최소 8GB, 16GB 이상 권장 | 충족 |

다운로드할 때 플랫폼으로 **macOS arm64**를 선택해야 합니다.

> 로컬 설치는 평가, 데이터 모델링, 테스트 및 100GB 미만의 소규모 마이그레이션에 적합합니다. 장시간 또는 대규모 운영 마이그레이션에는 소스 데이터베이스와 가까운 서버/VM 설치를 권장합니다.

## 1단계: 설치 전 준비

다음을 확인합니다.

- 소스 관계형 데이터베이스와 대상 MongoDB에 이 Mac에서 접속할 수 있어야 합니다.
- 방화벽에서 소스 및 대상 데이터베이스로 향하는 TCP 연결을 허용해야 합니다.
- 마이그레이션 중 Mac이 잠자기 상태가 되지 않도록 전원 설정을 조정합니다.
- MySQL, Oracle 또는 DB2를 소스로 사용한다면 아래의 JDBC 드라이버 추가 절차도 진행합니다.

## 2단계: 설치 파일 다운로드

1. [MongoDB Relational Migrator 공식 다운로드 페이지](https://www.mongodb.com/try/download/relational-migrator)를 엽니다.
2. 최신 버전을 선택합니다. 작성 시점의 최신 버전은 `1.18.0`입니다.
3. **Platform**에서 `macOS arm64`를 선택합니다.
4. **Package**에서 `dmg`를 선택합니다.
5. **Download**를 눌러 설치 파일을 받습니다.

버전 번호는 업데이트될 수 있으므로 특별한 호환성 요구가 없다면 페이지에 표시된 최신 버전을 사용합니다.

## 3단계: 애플리케이션 설치

1. Finder에서 `다운로드` 폴더를 엽니다.
2. `MongoDB.Relational.Migrator-<버전>.dmg` 파일을 더블 클릭합니다.
3. 열린 설치 창에서 **MongoDB Relational Migrator.app**을 **Applications** 폴더로 드래그합니다.
4. 복사가 완료되면 Finder 사이드바에서 설치 디스크를 추출합니다.

## 4단계: JDBC 드라이버 추가

소스 데이터베이스에 따라 필요한 작업이 다릅니다.

| 소스 데이터베이스 | JDBC 드라이버 |
| --- | --- |
| PostgreSQL | 기본 포함, 별도 설치 불필요 |
| SQL Server | 기본 포함, 별도 설치 불필요 |
| MySQL | Connector/J 9.1.x의 Platform Independent 버전 필요 |
| Oracle | Oracle 21c용 `ojdbc11.jar` 21.6.0.0 필요 |
| IBM DB2 | `db2jcc.jar` 3.x 또는 4.x 필요 |

MySQL, Oracle 또는 DB2를 사용하는 경우 다음 순서로 설치합니다.

1. 해당 공급자의 공식 페이지에서 드라이버를 다운로드합니다.
   - [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)
   - [Oracle JDBC 드라이버](https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html)
   - [IBM DB2 JDBC 드라이버](https://www.ibm.com/support/pages/db2-jdbc-driver-versions-and-downloads)
2. 압축 파일이라면 압축을 풀어 `.jar` 파일을 준비합니다.
3. Finder에서 `Shift + Command + G`를 누릅니다.
4. 다음 경로를 입력합니다.

   ```text
   /Library/Application Support/MongoDB/Relational Migrator/Drivers
   ```

5. `.jar` 파일을 해당 폴더에 복사합니다. 관리자 암호를 요구할 수 있습니다.
6. Relational Migrator가 이미 실행 중이라면 완전히 종료한 뒤 다시 실행합니다.

Oracle의 `XMLType` 컬럼을 마이그레이션할 경우에는 Oracle의 `xdb.jar`도 추가해야 합니다.

## 5단계: 최초 실행

1. Launchpad 또는 Finder의 `응용 프로그램` 폴더를 엽니다.
2. **MongoDB Relational Migrator**를 실행합니다.
3. macOS가 실행 확인을 요청하면 개발자와 다운로드 출처가 MongoDB인지 확인한 후 허용합니다.
4. 애플리케이션 화면이 열리면 설치가 완료된 것입니다.

## 6단계: 설치 확인

다음 항목을 확인합니다.

- Relational Migrator가 오류 없이 시작됩니다.
- 새 프로젝트를 생성하는 화면이 표시됩니다.
- 사용할 소스 데이터베이스 유형을 선택할 수 있습니다.
- 추가한 JDBC 드라이버가 필요한 데이터베이스 연결을 생성할 수 있습니다.

연결 테스트에는 다음 정보가 필요합니다.

- 소스 관계형 데이터베이스의 호스트, 포트, 데이터베이스명, 사용자명 및 암호
- 대상 MongoDB의 연결 문자열
- Atlas를 사용한다면 이 Mac의 IP가 Atlas Network Access 목록에 등록되어 있어야 함

## 주요 파일 위치

| 용도 | 경로 |
| --- | --- |
| 설정 파일 | `~/Library/Application Support/MongoDB/Relational Migrator/user.properties` |
| 추가 JDBC 드라이버 | `/Library/Application Support/MongoDB/Relational Migrator/Drivers` |
| 로그 파일 | `~/Library/Application Support/MongoDB/Relational Migrator/Logs/migrator.log` |

## 문제 해결

### 앱이 열리지 않는 경우

1. 설치 파일을 MongoDB 공식 다운로드 페이지에서 받았는지 확인합니다.
2. `시스템 설정 > 개인정보 보호 및 보안`에서 차단 알림을 확인합니다.
3. 출처가 MongoDB임을 확인한 뒤 **확인 없이 열기** 또는 **그래도 열기**를 선택합니다.

### MySQL, Oracle 또는 DB2 연결 옵션에서 드라이버 오류가 발생하는 경우

1. `.jar` 파일이 정확한 `Drivers` 경로에 있는지 확인합니다.
2. 드라이버 버전이 위 표의 요구 사항과 일치하는지 확인합니다.
3. Relational Migrator를 완전히 종료하고 다시 실행합니다.

### 실행 또는 연결 오류를 조사하는 경우

다음 로그 파일을 확인합니다.

```text
~/Library/Application Support/MongoDB/Relational Migrator/Logs/migrator.log
```

Finder에서 `Shift + Command + G`를 누르고 위 경로를 입력하면 파일 위치로 이동할 수 있습니다.

## 현재 설치된 로컬 MySQL 샘플 환경

2026-08-21에 Homebrew를 사용해 다음 환경을 구성했습니다.

| 항목 | 값 |
| --- | --- |
| MySQL Server | 8.4.11 LTS |
| 호스트 | `127.0.0.1` |
| 포트 | `3306` |
| 데이터베이스 | `world` |
| 사용자 | `world_migrator` |
| 비밀번호 | `WorldMigrator_2026!` |
| JDBC URL | `jdbc:mysql://127.0.0.1:3306/world` |

이 계정은 로컬 샘플 및 Relational Migrator 실습 전용입니다. 운영 환경에서 같은 비밀번호를 재사용하지 않습니다.

샘플 데이터는 다음과 같이 구성되어 있습니다.

| 테이블 | 행 수 |
| --- | ---: |
| `city` | 4,079 |
| `country` | 239 |
| `countrylanguage` | 984 |

MySQL은 로그인할 때 자동으로 시작되며 외부 네트워크에 노출되지 않도록 `127.0.0.1`에서만 연결을 받습니다. 설정 파일은 `/opt/homebrew/etc/my.cnf`입니다.

서비스 상태 확인:

```bash
brew services list
```

서비스 시작, 중지 및 재시작:

```bash
brew services start mysql@8.4
brew services stop mysql@8.4
brew services restart mysql@8.4
```

터미널에서 `world` 데이터베이스에 접속:

```bash
/opt/homebrew/opt/mysql@8.4/bin/mysql \
  -h 127.0.0.1 \
  -P 3306 \
  -u world_migrator \
  -p \
  world
```

비밀번호 입력 메시지가 표시되면 `WorldMigrator_2026!`를 입력합니다.

MySQL `root` 비밀번호는 macOS Keychain에 임의의 강력한 값으로 저장했습니다. 필요한 경우 다음 명령으로 확인할 수 있습니다.

```bash
security find-generic-password \
  -a root \
  -s "Homebrew MySQL 8.4 root" \
  -w
```

## 참고 자료

- [공식 macOS 설치 문서](https://www.mongodb.com/docs/relational-migrator/installation/install-on-a-local-machine/install-mac/)
- [공식 시스템 요구 사항](https://www.mongodb.com/docs/relational-migrator/installation/system-requirements/)
- [공식 다운로드 페이지](https://www.mongodb.com/try/download/relational-migrator)
- [설치 및 배포 모델 안내](https://www.mongodb.com/docs/relational-migrator/installation/)
