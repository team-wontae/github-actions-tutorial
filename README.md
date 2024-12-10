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
