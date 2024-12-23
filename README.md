# GITHUB ACTIONS

This has been edited on behalf of 05-1 action.

### Context

- Access information about runs, variables, jobs and much more → sources of data

- We can provide all the necessary information to our workloads
  Not every context is available at every step of the job

- There are many contexts like github, env, inputs, vars, secrets, matrix etc. Fortunately, with the help of VScode Github action extension, it tells us which ones we could use.

### Variable

- Single workflow variable

  - workflow, job and step level

    ```bash
    echo "Hello $NAME!"
    ```

- Multiple workflows variable

  - organization, repository and enviornment level

    ```bash
    echo "Hello ${{vars.NAME}}!"
    # vars context으로 접근해야 한다.
    ```

    ![Environment, Repository and Organization variables](https://github.com/user-attachments/assets/22633e94-5416-4ff5-8693-d07827885df5)

    - Organization 만들고, organization value를 만들기 위해 리포지토리를 organization으로 옮김
    - Repository variables은 Repo에서 설정가능
    - Enviornment variables은 dev, prod, staging같은 각각의 환경에서 쓰이는 변수

### Function

- General purpose functions(methods)

  - utility functions to interact with data from contexts, and model more complex behavior like more advanced control workflow and job execution
    ```
    contains()
    startsWith()
    endsWith()
    fromJSON() → Deserialize, env변수를 가져올때도 사용한다
    toJSON() → Serialize
    hashFiles()
    ...
    ```

- Status check functions
  - Functions that can use the status of the workflow, previous jobs or steps to define whether a certain job or step should be executed
    ```
    success() → previous jobs or steps all succeeded?
    failure() → any of them failed?
    cancelled() →
    always()
    ```

PR trigger해서 github.context.event.pull_request invoke하기

### Artifacts

- Stored for up to 90 days
- Managed via 2 actions
  - upload-artifact
  - download-artifact
    - 같은 workflow에서 업로드된 artifact만 다운로드 가능하다
- Recommended when the stored files are likely to be accessed outside the workflow, for example
  - Build outupus
  - Test results
  - Logs
  - Uploading and downloading the build outputs for deployments

### Caching

- Stored for up to 7 days
- Managed via a single action
  - cache
- Recommended when the stored files are likely to be accessed only within the workflow, for example
  - Build dependencies

### Matrices

- Job level에서 strategy.matrix로 정의한다
  - jobs.job_name.strategy.matrix
  - Cartesian product [] X []
- Don't have to copy the same job over and over again

  - Run several variations of the same job
  - 카르테시안 곱의 수만큼의 job이 실행된다.

  - provide key-value pairs
  - each key will become available at the matrix context

- On private repositories, it might surge the billing, so be careful

```yml
jobs:
  backwards-compatibility:
    name: ${{matrix.os}}-${{matrix.node}}
    strategy:
      fail-fast: false
      matrix:
        node: [14, 16, 18] # keys
        os: [ubuntu-latest, macos-latest] # values
    runs-on: ${{matrix.os}}
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: ${{matrix.node}}
```

- Fail fast strategy
  - 카르테시안 곱의 job들 중에 하나라도 실패하면, 나머지 job들이 cancel되는 것
  - strategy에서 정의
    - true(default) or false

### Environments

Create multiple environments with different rules for multiple deployments

- Staging, UAT(User acceptance testing), Prod 등 다양한 environemnt를 만들 수 있다

- Required approvers를 줘서 manual approvals을 줄 수도 있고, branche restrictions도 가능하다.

- Private Repo에서는 빌링이 발생할수도 있다.
