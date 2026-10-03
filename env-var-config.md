Add environment-based configuration to this Go project using github.com/caarlos0/env/

Requirements
1. Create internal/config/config.go with:
   - A typed `Config` struct using `env` struct tags and these fields only:
     - AppName (APP_NAME, default "app")
     - AppEnv (APP_ENV, default "local")
     - HTTPPort (HTTP_PORT, default 8080)
     - HTTPReadHeaderTimeout (HTTP_READ_HEADER_TIMEOUT,default 30)
     - HTTPReadTimeout (HTTP_READ_TIMEOUT,default 30)
     - HTTPWriteTimeout (HTTP_WRITE_TIMEOUT, default 30)
     - HTTPIdleTimeout (HTTP_IDLE_TIMEOUT, default 60)
     - HTTPShutdownTimeout (HTTP_SHUTDOWN_TIMEOUT, default 60)
     - LogLevel (LOG_LEVEL, default error) // debug,info,warn,error
     - LogFormat (LOG_FORMAT, default json) // auto, json, console
     - LogOutput (LOG_OUTPUT, default stdout) // stdout,stderr
     - RequestTimeout (REQUEST_TIMEOUT, default 20)
     - MaxBodyBytes (MAX_BODY_BYTES, default 1048576)
     - LogBody (LOG_BODY, default false)
     - LogRequestBodyMaxBytes (LOG_REQUEST_BODY_MAX_BYTES, default 1024)
     - LogResponseBodyMaxBytes (LOG_RESPONSE_BODY_MAX_BYTES, default 1024)
     - CORSOrigins (CORS_ORIGINS, default: "*")
     - CORSAllowCredentials (CORS_ALLOW_CREDENTIALS, default false)
     - CORSMaxAge (CORS_MAX_AGE, default 5m)
     - CORSMethods (CORS_METHODS, default "GET,POST,PUT,PATCH,DELETE,HEAD")
     - CORSHeaders (CORS_HEADERS, default "Accept,Authorization,Content-Type,X-Request-Id")
     - CORSExposedHeaders (CORS_EXPOSED_HEADERS, default "X-Request-Id")
     - AccessLogSkip (ACCESS_LOG_SKIP, default "")
   - parse without generics
   // Example
      func Load() (Config, error) {
      	var cfg Config
      	err := env.Parse(&cfg)
      	if err != nil {
      		return Config{}, err
      	}
      	if err := cfg.validate(); err != nil {
      		return Config{}, err
      	}
      	return cfg, nil
      }

2. Create .env.example listing the three variables with placeholder values and a one-line comment each. The app does not read this file itself.
3. Write internal/config/config_test.go covering: defaults applied when nothing is set, values overridden by env, invalid AppEnv, invalid port, and an unparsable port (e.g. "abc"). Use t.Setenv for isolation.
4. Wire it into entry point main file: call config.Load() first, and on error print the message to stderr and exit with a non-zero status. 
