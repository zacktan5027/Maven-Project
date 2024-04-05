name: cicd
on: push
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: clone the repository
        uses: actions/checkout@v4.1.2
      - name: Setup Java 11
        uses: actions/setup-java@v4.2.1
        with: 
          java-version: '11'
          distribution: 'temurin'
          cache: 'maven'
      - name: Create package using maven
        run: mvn ddd package
      - name: Setup Qemu
        uses: docker/setup-qemu-action@v3
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: create docker image and container
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - name: Build and push
        run: |
          docker build -t gagandeepthinknyx/intelus:latest .
          docker push gagandeepthinknyx/intelus:latest
      - name: Publish Success messages to Slack
        uses: rtCamp/action-slack-notify@v2
        env:
           SLACK_WEBHOOK: ${{ secrets.slack_secret }}
           SLACK_COLOR: ${{ job.status }}
           SLACK_TITLE: 'YAY! ITS A SUCCESS'
           SLACK_MESSAGE: 'Build completed for ${{ GITHUB.WORKFLOW }} number ${{ GITHUB.RUN_NUMBER }}'
      - name: Publish Failure messages to Slack
        if: ${{ failure() }}
        uses: rtCamp/action-slack-notify@v2
        env:
           SLACK_WEBHOOK: ${{ secrets.slack_secret }}
           SLACK_COLOR: ${{ job.status }}
           SLACK_TITLE: 'ALERT! BUILD JOB FAILED'
           SLACK_MESSAGE: 'Build failed for ${{ GITHUB.WORKFLOW }} number ${{ GITHUB.RUN_NUMBER }}'

          
        
        
