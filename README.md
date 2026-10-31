# bongoDev Incident Management — Ubuntu EC2 (No Docker)

A hands-on deployment project for running a React + Express + PostgreSQL incident-management application on Ubuntu 24.04 EC2 using Node.js, Nginx, systemd, and optional GitHub Actions CI/CD — without Docker.

## Project overview

The application is deployed natively on an Ubuntu Server 24.04 LTS EC2 instance:

- **Frontend:** React + Vite, built and served by Nginx
- **Backend:** Node.js + Express REST API, managed with systemd
- **Database:** PostgreSQL
- **Web server / reverse proxy:** Nginx
- **Automation:** Bash deployment scripts and an optional GitHub Actions workflow

## Repository structure

| Path | Purpose |
|---|---|
| `frontend/` | React + Vite application |
| `backend/` | Express API and PostgreSQL integration |
| `scripts/install-ubuntu.sh` | Install required Ubuntu packages and services |
| `scripts/install-nodejs-ubuntu.sh` | Install Node.js 24 |
| `scripts/install-postgresql-ubuntu.sh` | Install PostgreSQL |
| `scripts/install-nginx-ubuntu.sh` | Install Nginx |
| `scripts/setup-database.sh` | Set up the database and local backend environment |
| `scripts/deploy-ubuntu.sh` | Build the frontend and configure the app service and Nginx |
| `scripts/check-services.sh` | Check service and port status |
| `.github/workflows/deploy.yml` | Optional GitHub Actions deployment workflow |
| `docs/GITHUB_ACTIONS.md` | CI/CD setup and deployment guide |

## Manual deployment

Clone the repository on your Ubuntu 24.04 EC2 instance:

```bash
git clone https://github.com/tanahmeeedsx/incident-management-2.git
cd incident-management-2

bash scripts/install-ubuntu.sh
bash scripts/setup-database.sh
bash scripts/deploy-ubuntu.sh
bash scripts/check-services.sh
```

After deployment, open `http://YOUR_EC2_PUBLIC_IP/` in your browser. Verify the API locally with:

```bash
curl -f http://127.0.0.1/health
```

See the scripts and application configuration for environment-specific values.

## Optional CI/CD with GitHub Actions

The workflow in `.github/workflows/deploy.yml` can build the application and deploy updates to an already-configured EC2 instance after a push to `main`. It does not create or provision EC2 infrastructure.

Follow [docs/GITHUB_ACTIONS.md](docs/GITHUB_ACTIONS.md) to configure the required repository variables, secrets, and SSH access. The manual deployment should work before enabling automated deployment.

## Safety notes

- This is an educational demo and does not include authentication. It uses HTTP; do not use it for sensitive incident data.
- Do not expose PostgreSQL port `5432` or the backend port `5000` publicly. Allow web traffic through Nginx and restrict SSH access to trusted sources.
- Never commit `backend/.env`, database passwords, or SSH private keys.
- Terminate demo EC2 resources when finished to avoid ongoing AWS charges.
