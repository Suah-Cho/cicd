# cicd
CICD 테스트 레포지터리

```plantuml

@startuml

actor dev

rectangle "Github" {
    rectangle "Frontend Repo"
    rectangle "Backend Repo"
    rectangle cicd_Repo [
        cicd repo
        ---
         ㄴ.github/workflows/deploy.yml
         ㄴ/docker-compose.yml
    ]

    "Frontend Repo" --> cicd_Repo : 3. image tag change and action trigger 
    "Backend Repo" --> cicd_Repo : 3. image tag change and action trigger
}

cloud "Azure" {
    rectangle "ACR" {
        rectangle "Frontend-ACR"
        rectangle "Backend-ACR"
    }

    rectangle "Azure VM" {
        rectangle "docker-compose" {
            rectangle "frontend container"
            rectangle "backend container"
        }
    }
}

dev --> "Frontend Repo" : 1. main branch push
dev --> "Backend Repo" : 1. main branch push

"Frontend Repo" ---> "Frontend-ACR" : 2. image upload
"Backend Repo" ---> "Backend-ACR" : 2. image upload

"cicd_Repo" --> "Azure VM" : 4. git pull

"Frontend-ACR" --> "frontend container" : 5. image pull
"Backend-ACR" --> "backend container" : 5. image pull

@enduml

```