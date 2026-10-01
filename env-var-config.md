Add environment-based configuration to this Go project using github.com/caarlos0/env/

Requirements
1. Create internal/config/config.go with:
   - A typed `Config` struct using `env` struct tags and these fields only:
     - AppName (APP_NAME, default "app")
     - AppEnv (APP_ENV, default "local")
     - HTTPPort (HTTP_PORT, default 8080)
2. Create .env.example listing the three variables with placeholder values and a one-line comment each. The app does not read this file itself.
3. Write internal/config/config_test.go covering: defaults applied when nothing is set, values overridden by env, invalid AppEnv, invalid port, and an unparsable port (e.g. "abc"). Use t.Setenv for isolation.
4. Wire it into cmd/api/main.go: call config.Load() first, and on error print the message to stderr and exit with a non-zero status. 
