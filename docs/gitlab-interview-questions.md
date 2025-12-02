# **GitLab CI/CD — Essential Interview Cheat-Sheet**

## **1. Pipeline Basics**

* `.gitlab-ci.yml` defines the pipeline.
* Pipeline = **stages → jobs → scripts**.
* Default stages: `build`, `test`, `deploy`, but you can name your own.
* Jobs run in **parallel** inside the same stage.
* Stages run **sequentially**, unless you configure dependencies differently.

## **2. Runners**

* Runner executes jobs.
* Types: **shared**, **group**, **project**, **custom**.
* Executors: **Docker**, **Shell**, **Kubernetes**, **VM**, **Docker Machine**.
* Docker executor is the most common — isolated, reproducible builds.
* Kubernetes executor is used for scalable, cloud-native pipelines.

## **3. Caching & Artifacts**

* **cache:** speeds up pipeline by reusing dependencies.
* **artifacts:** files saved after a job (e.g., build outputs, test reports).
* Artifacts can be passed between stages using `dependencies:` or `needs:`.

## **4. Rules, Only/Except**

* `rules:` is the modern way — more flexible and expressive.
* `only/except:` is older, simpler, but limited.
* Use `rules:` for conditions like `$CI_COMMIT_BRANCH == "main"` or file changes.

## **5. Variables**

* Defined in YAML or GitLab UI.
* Useful for secrets, environment URLs, or config flags.
* Masked variables hide values in job logs.
* `CI_***` built-in variables give metadata about commit, branch, pipeline, etc.

## **6. Environments & Deployments**

* **environment:** defines where deployment goes (staging, prod).
* Supports:

  * **Manual deployments**
  * **Timed deployments**
  * **Stop actions**
* With `environment:url`, GitLab creates an auto “Open deployment” button.
* Protected environments restrict who can deploy to production.

## **7. Artifacts & Dependencies**

* Use `artifacts: paths:` for files needed by later stages.
* Use `dependencies:` to pull artifacts from specific jobs.
* Use `needs:` for directed-acyclic job graph + parallelized stages.

## **8. Pipeline Optimization**

* Use `cache:` to speed up builds.
* Use `needs:` to break strict stage ordering.
* Use `rules:` + `changes:` to skip irrelevant jobs.
* Use **Docker layer caching** to speed up image builds.
* Use **parent-child pipelines** to split large pipelines.

## **9. Schedules**

* GitLab supports scheduled pipelines (CRON-like).
* Useful for nightly builds, security scans, backups, lint checks.

## **10. Includes**

* `include:` allows splitting CI logic into multiple files.
* Very useful for large pipelines or reusable templates.
* Can include by local file, remote URL, or project.

## **11. Security Scanning**

* GitLab provides built-in scanning:

  * **SAST** (static code)
  * **DAST** (dynamic)
  * **Dependency Scanning**
  * **Container Scanning**
* Enabled via `include: security/*` templates.

## **12. Artifacts Expiration**

* `expire_in:` sets how long artifacts persist.
* Helps reduce storage cost.

## **13. Protected Branches**

* Jobs that deploy to production often require protected branches.
* Only users with permission can run those jobs.

## **14. Manual & Delayed Jobs**

* `when: manual` → requires human approval.
* `when: delayed` → run job after a delay (e.g., 10 minutes).

## **15. Parallel Jobs**

* `parallel:` spins multiple instances of the same job (matrix builds).
* Common for test sharding and multi-platform builds.

## **16. DAG Pipelines (Directed Acyclic Graph)**

* Achieved with `needs:`.
* Faster pipelines because jobs run as soon as dependencies are ready.

## **17. Retry Strategy**

* `retry:` automatically retries failed jobs.
* Useful for flaky tests or network issues.

## **18. Artifacts Reports**

* Build special reports: JUnit, coverage, dependency scans, etc.
* These appear nicely in the GitLab UI.

## **19. Pipeline Types**

* **Branch pipeline**
* **Merge request pipeline**
* **Tag pipeline**
* **Trigger pipeline**
* **Child/parent pipeline**
* **Multi-project pipeline**

## **20. Deploy Tokens, Registry, and Images**

* GitLab has built-in **Container Registry**.
* Use `CI_REGISTRY` and `CI_JOB_TOKEN` to push/pull images safely.
* Deploy tokens can allow external systems to pull images.

