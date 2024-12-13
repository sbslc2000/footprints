```yaml
backend: 
	image: peteseo/complex-backend 
	container_name: app_backend 
	volumes: 
		- /app/node_modules 
		- ./backend:/app 
	deploy: 
		resources: 
			limits: 
				memory: 128m
```
