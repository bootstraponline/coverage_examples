# coverage_examples

## Languages & Coverage tools

- Go
  - https://github.com/codecov/example-go
- Python
  - https://pypi.org/project/coverage/
  - https://github.com/codecov/example-python
- Java & Kotlin
  - https://github.com/jacoco/jacoco
  - https://github.com/codecov/example-gradle (Android)
- JavaScript & TypeScript
  - https://github.com/istanbuljs
  - https://github.com/codecov/example-node
- Swift
  - https://github.com/SlatherOrg/slather
  - https://github.com/codecov/example-swift (iOS)

## Generate Coverage

Steps to generate coverage for each supported language.

#### Go

Install [gocover-cobertura](https://github.com/t-yuki/gocover-cobertura)

```
$ go get golang.org/x/tools/cmd/cover
$ go get github.com/t-yuki/gocover-cobertura
```

```
cd example-go
go test -race -coverprofile=coverage.txt -covermode=atomic
gocover-cobertura < coverage.txt > coverage.xml
```

Go native coverage is `coverage.txt` and cobertura is `coverage.xml`

#### Python

```
pip3 install coverage
coverage run tests.py
coverage xml
```

`.coverage` from coverage run is the native coverage format.
`coverage.xml` from `coverage xml` is cobertura.

#### Java & Kotlin

```
cd example-gradle
./gradlew check
./gradlew jacocoTestReport
```

Coverage file is in `./build/reports/jacoco/test/jacocoTestReport.xml`

#### JavaScript & TypeScript

```
cd example-node
cd istanbul-mocha
npm install .
```

Create `.istanbul.yml` with:

```
reporting:
  print: summary
  reports:
    - cobertura
```

Run `npm run test`

`./coverage/coverage.json` is the native format
`./coverage/cobertura-coverage.xml` is the cobertura format.

#### Swift

Install and configure Ruby.

`brew install ruby`

```
# ruby
export PATH="/usr/local/opt/ruby/bin:$PATH"
# Based on `gem environment gemdir`
export PATH="/usr/local/lib/ruby/gems/3.0.0/bin:$PATH"
```

Run tests and generate coverage.

```
cd example-swift
gem install slather
open SwiftExample.xcodeproj/
```

- `Product` -> `Scheme` -> `Edit Scheme`.
- Click on the `Test` target and select `Options...`
- Verify the `Code Coverage` box is checked and it says `Gather coverage for all targets`
- Add new Scheme `SwiftExampleTests` and ensure the `Shared` box is checked
- Enable coverage for the newly added scheme

Run the tests:

```
xcodebuild test -scheme SwiftExampleTests -destination 'platform=iOS Simulator,name=iPhone 12,OS=latest' -derivedDataPath output
```

Process coverage

```
slather coverage -x --build-directory output --scheme SwiftExampleTests --binary-basename SwiftExample SwiftExample.xcodeproj
```

The coverage will be in `cobertura.xml`
