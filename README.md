# 📊 GitHub Issue Tracker & Forecasting Platform

A full-stack cloud-native application that tracks the number of **created and closed issues** for GitHub repositories over the past year and **predicts future trends** using **LSTM + Prophet** time series forecasting. The platform is deployed as three microservices (React, Flask, and ML-based Forecast) using Docker and Google Cloud Run.

---

## 🧩 Project Architecture

```
React (Frontend)
    ↓ fetch /api
Flask Microservice (Backend API)
    ↓ calls ML
Forecast Microservice (LSTM/Prophet)
    → Google Cloud Storage (image hosting)
```

- **React** handles user interaction, displays bar charts and forecasted images.
- **Flask** fetches GitHub issue data, processes it, and forwards it to the ML model.
- **Forecast (LSTM)** processes historical issue data, forecasts future trends, and returns image URLs from GCP.

---

## 🚀 Features

- 📥 Fetches GitHub issue data (created/closed) from the past year
- 📊 Renders bar charts of historical issue trends using Highcharts
- 🔮 Predicts future issue trends using LSTM + Prophet
- 🖼️ Displays GCP-hosted forecast plots
- ☁️ Fully containerized and deployable to Google Cloud Run

---

## 🛠 Tech Stack

| Layer      | Tech                           |
|------------|--------------------------------|
| Frontend   | React, Highcharts, MUI         |
| Backend    | Flask, GitHub API, requests    |
| Forecast   | TensorFlow, Keras, Prophet     |
| Cloud      | Google Cloud Run, GCS, Docker  |
| Other      | GitHub Token Auth, Proxy API   |

---

## 📦 Microservices Breakdown

### 1. 🔹 React (Frontend)
- Takes GitHub repo name (e.g. `angular/material`) as input
- Displays issue statistics and forecast images
- Uses `setupProxy.js` to route requests to Flask

```bash
cd react/
npm install
npm start
```

> 🔧 Update `src/setupProxy.js` with your Flask service URL (`http://localhost:5000` for local dev)

---

### 2. 🔹 Flask Microservice
- Accepts repo name via API call from React
- Fetches issue data using GitHub API
- Sends preprocessed data to Forecast microservice

```bash
cd flask/
python -m venv env
source env/bin/activate  # or env\Scripts\activate.bat on Windows
pip install -r requirements.txt
python app.py
```

> 🔧 Set the `GITHUB_TOKEN` as an environment variable or use `.env`

---

### 3. 🔹 Forecast Microservice (LSTM + Prophet)
- Accepts JSON issue data from Flask
- Applies time series forecasting (LSTM, Prophet)
- Saves forecast plots to Google Cloud Storage

```bash
cd forecast/
python -m venv env
source env/bin/activate
pip install -r requirements.txt
python app.py
```

> 🔧 Set required environment variables:
```env
BUCKET_NAME=your-gcs-bucket-name
BASE_IMAGE_PATH=https://storage.googleapis.com/your-bucket/
```

---

## ⚙️ Environment Variables

Create a `.env` file (not committed to GitHub) with:

```env
# For Flask
GITHUB_TOKEN=your_github_pat

# For Forecast
BUCKET_NAME=your-gcs-bucket
BASE_IMAGE_PATH=https://storage.googleapis.com/your-gcs-bucket/
```

---

## 🐳 Docker & Cloud Run Deployment

Each microservice is dockerized for easy deployment to **Google Cloud Run**.

### Example Steps (Repeat per service):

```bash
docker build -t gcr.io/your-project-id/your-service-name .
docker push gcr.io/your-project-id/your-service-name
gcloud run deploy --image gcr.io/your-project-id/your-service-name --platform managed
```

> ⚠️ Remember to set min instances to 1, allow unauthenticated access, and set proper environment variables via GCP Console.

---

## 🧪 Testing

React tests use **Jest** and **React Testing Library**.

```bash
npm test
```

---

## 📂 Project Structure

```
├── react/
│   ├── src/
│   ├── public/
│   ├── Dockerfile
│   └── package.json
├── flask/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
├── forecast/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
```

---

## 📌 Prerequisites

Make sure you have the following installed:
- Docker
- Google Cloud SDK
- Node.js (v14+)
- Python 3.8+
- GitHub Personal Access Token (with `repo` scope)

---

## 🤝 Contributing

Feel free to fork, open issues, or submit pull requests.

---

## 📜 License

This project is licensed under the MIT License.

---

## 🙋‍♂️ Author

Built with ❤️ by [Your Name]  
[LinkedIn](https://linkedin.com/in/your-link) · [GitHub](https://github.com/your-username)
