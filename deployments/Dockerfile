# deployments/Dockerfile
FROM golang:1.22-alpine AS builder

WORKDIR /app

# 의존성 설치
COPY go.mod go.sum ./
RUN go mod download

# 소스 코드 복사
COPY . .

# 애플리케이션 빌드
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o server ./cmd/server

# 최종 이미지
FROM alpine:3.19

RUN apk --no-cache add ca-certificates

WORKDIR /root/

# 빌더 스테이지에서 바이너리 복사
COPY --from=builder /app/server .
COPY --from=builder /app/configs ./configs

# 포트 설정
EXPOSE 8080

# 환경 변수 설정
ENV GIN_MODE=release

# 애플리케이션 실행
CMD ["./server"]