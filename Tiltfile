print("""
-----------------------------------------------------------------
✨ Hello Tilt! This appears in the (Tiltfile) pane whenever Tilt
   evaluates this file.
-----------------------------------------------------------------
""".strip())

# Build Docker image
#   Tilt will automatically associate image builds with the resource(s)
#   that reference them (e.g. via Kubernetes or Docker Compose YAML).
#
#   More info: https://docs.tilt.dev/api.html#api.docker_build
#


k8s_yaml(kustomize('k8s'))
k8s_resource('tenement-controller',
             port_forwards=['3000:3000'],
             labels=['tenement-controller'],
)

k8s_resource('frontend',
             port_forwards=['5173:5173'],
             labels=['frontend'],
)

docker_build(
  'tenement-controller',
  context='./controller',
  target='dev',
  only=['src','Cargo.toml','Cargo.lock'],
  live_update=[
    sync('./controller/src', '/tenement-controller/src'),
    sync('./controller/Cargo.toml', '/tenement-controller/Cargo.toml'),
    sync('./controller/Cargo.lock', '/tenement-controller/Cargo.lock'),
  ])

docker_build(
  'frontend',
  context='./webui',
  target='dev',
  only=['package.json', 'package-lock.json', 
    'src', 'public',
    'vite.config.ts', 'tsconfig.json'
  ],
  live_update=[
    sync('./webui/src', '/app/src'),
    sync('./webui/public', '/app/public'),
    sync('./webui/package.json', '/app/package.json'),
    sync('./webui/package-lock.json', '/app/package-lock.json'),
    sync('./webui/vite.config.ts', '/app/vite.config.ts'),
  ])
