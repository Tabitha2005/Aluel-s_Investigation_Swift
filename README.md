# Aluel's Investigation Swift

![Aluel's Investigation Swift Banner](Aluel.webp)  
*A platform to help you recover misplaced belongings, reunite found items with their rightful owners, and access emergency services.*

## 📖 Project Overview

The **Aluel's Investigation Swift** is a web application designed to address the real-world problem of losing or finding items by providing a platform where users can report misplaced or found items, search for matches, and visualize item locations on an interactive map. Built with a focus on user experience, the app combines intuitive design with powerful features to connect communities and solve everyday challenges. Additionally, it includes an innovative **Emergency AI** feature that provides real-time assistance by offering emergency contact numbers and locating nearby services (e.g., hospitals, police stations).

This project was developed as part of a university assignment to create a meaningful application that utilizes external APIs, enables user interaction with data, and is deployed on web servers with a load balancer. The app goes beyond basic functionality by offering practical value, robust error handling, and performance optimizations like API caching.

### ✨ Key Features

- **Submit Items to Aluel's Investigation Swift**: Effortlessly document items you have misplaced or discovered, complete with comprehensive descriptions, including title, date, location, contact info, and optional images.
- **Search and Filter**: Search for items by keyword (e.g., "keys") and filter by status ("misplaced" or "found") to quickly find matches.
- **Interactive Map**: View item locations on a Leaflet-powered map, with popups showing item details and a list of pinned addresses.
- **Emergency AI**: A real-time chat assistant that provides emergency contact numbers (e.g., police, ambulance) and locates nearby services using the Geoapify API.
- **Responsive Design**: Access the app seamlessly on any device—desktop, tablet, or mobile.
- **Error Handling**: Comprehensive error handling for API failures, with user-friendly feedback via alerts and chat messages.
- **Performance Optimization**: API caching for geocoding requests to reduce load times and respect rate limits.

### 🎥 Demo Video

Watch a demo of Aluel's Investigation Swift in action:  
[Demo Video Link](https://www.canva.com/design/DAGjEGZZEtc/qrq_m5qKzdv_T5j89KdMmg/watch?utm_content=DAGjEGZZEtc&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=hab1200133f)

## 🛠️ Tech Stack

- **Frontend**: HTML, CSS, JavaScript
- **Framework**: Vite (for building and bundling)
- **APIs**:
    - Geoapify API (for Emergency AI)
    - Nominatim API (for geocoding item locations)
    - Mapbox API (fallback for geocoding)
    - Firebase Firestore (for storing and retrieving misplaced/found items)
- **Libraries**:
    - Leaflet (for interactive maps)
- **Deployment**: Nginx web servers (Web01, Web02) with a load balancer (Lb01)

## 🚀 Local Setup Instructions

Follow these steps to set up and run Aluel's Investigation Swift on your local machine.

### Prerequisites

- **Node.js** (v16 or higher) and **npm** installed on your machine.
- A Firebase project set up with Firestore enabled.
- API keys for:
    - Geoapify (for Emergency AI)
    - Mapbox (for geocoding fallback)

### Steps

1. **Clone the Repository**  
     Clone this repository to your local machine:
     ```bash
     git clone https://github.com/your-username/aluel-investigation-swift.git
     cd aluel-investigation-swift
     ```

2. **Install Dependencies**  
     Install the required npm packages:
     ```bash
     npm install
     ```

3. **Set Up Environment Variables**  
     Create a `.env` file in the project root and add the following environment variables:
     ```env
     VITE_GEOAPIFY_API_KEY=your-geoapify-api-key-here
     VITE_MAPBOX_API_KEY=your-mapbox-api-key-here
     VITE_FIREBASE_API_KEY=your-firebase-api-key-here
     VITE_FIREBASE_AUTH_DOMAIN=your-firebase-auth-domain
     VITE_FIREBASE_PROJECT_ID=your-firebase-project-id
     VITE_FIREBASE_STORAGE_BUCKET=your-firebase-storage-bucket
     VITE_FIREBASE_MESSAGING_SENDER_ID=your-firebase-messaging-sender-id
     VITE_FIREBASE_APP_ID=your-firebase-app-id
     VITE_FIREBASE_MEASUREMENT_ID=your-firebase-measurement-id
     ```
     Replace the placeholders with your actual API keys and Firebase configuration values.  
     **Note:** Do not commit the `.env` file to GitHub. It’s already included in `.gitignore` to prevent accidental exposure of sensitive information.

4. **Run the Development Server**  
     Start the Vite development server:
     ```bash
     npm run dev
     ```
     The app will be available at `http://localhost:3000`. Open this URL in your browser to interact with the app locally.

5. **Build for Production (Optional)**  
     To create a production-ready build:
     ```bash
     npx vite build
     ```
     The built files will be in the `dist` directory. You can preview the production build locally:
     ```bash
     npx vite preview
     ```

## 🌐 Deployment Instructions

Aluel's Investigation Swift is deployed on two web servers (Web01 and Web02) with a load balancer (Lb01) to distribute traffic. Below are the steps to replicate the deployment.

### Prerequisites

- Two web servers (e.g., Web01 and Web02) running Ubuntu with Nginx installed.
- A load balancer (e.g., Lb01) running Ubuntu with haproxy configured for load balancing.
- SSH access to all servers.
- A domain name (e.g., `www.aluel-tech.com`) pointing to the load balancer’s IP address.

### Deployment Steps

1. **Build the Project Locally**  
     On your local machine, build the project to generate the `dist` directory:
     ```bash
     npx vite build
     ```

2. **Commit the Built Files to GitHub**  
     The `dist` directory is included in the repository for deployment purposes:
     ```bash
     git add dist
     git commit -m "Add production build for deployment"
     git push origin main
     ```

3. **Deploy to Web01**  
     SSH into Web01 and deploy the app:
     ```bash
     ssh ubuntu@<web01-ip>
     cd /home/ubuntu
     git clone https://github.com/your-username/aluel-investigation-swift.git
     cd aluel-investigation-swift
     sudo cp -r dist/* /var/www/html/
     sudo chmod -R 644 /var/www/html/*
     sudo chown -R www-data:www-data /var/www/html
     ```

4. **Deploy to Web02**  
     Repeat the process on Web02:
     ```bash
     ssh ubuntu@<web02-ip>
     cd /home/ubuntu
     git clone https://github.com/your-username/aluel-investigation-swift.git
     cd aluel-investigation-swift
     sudo cp -r dist/* /var/www/html/
     sudo chmod -R 644 /var/www/html/*
     sudo chown -R www-data:www-data /var/www/html
     ```

5. **Configure the Load Balancer (Lb01)**  
     Update the haproxy configuration on Lb01:
     ```nginx
     upstream backend {
             server <web01-ip>:80;
             server <web02-ip>:80;
     }

     server {
             listen 80;
             server_name www.aluel-tech.com;

             location / {
                     proxy_pass http://backend;
                     proxy_set_header Host $host;
                     proxy_set_header X-Real-IP $remote_addr;
                     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                     proxy_set_header X-Forwarded-Proto $scheme;
             }
     }
     ```
     Test and reload haproxy:
     ```bash
     haproxy -c -f /etc/haproxy/haproxy.cfg
     sudo service haproxy restart
     ```

6. **Test the Deployment**  
     Access the app at `https://www.aluel-tech.com`. Verify that all features work as expected.

### Security Notes

- **API Keys**: Restrict API keys to the domain `https://www.aluel-tech.com` in their respective dashboards.
- **Firebase Security**: Firestore security rules are set to allow public reads but restrict writes to authenticated users.

## 📜 API and Resource Attribution

Aluel's Investigation Swift relies on several external APIs and libraries. We are grateful to the developers and organizations behind these resources:

- **Geoapify API**: Used for geocoding and finding nearby emergency services.
- **Nominatim API**: Used for geocoding item locations.
- **Mapbox API**: Fallback geocoding service.
- **Firebase Firestore**: Cloud database for storing and retrieving misplaced/found items.
- **Leaflet**: JavaScript library for interactive maps.
- **OpenStreetMap**: Map tiles for the Leaflet map.
- **Vite**: Build tool for bundling and serving the app.

## 📝 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## 👤 About the Creator

I’m Aluel Tabitha Kuir Bior, a passionate developer from South Sudan. With expertise in full-stack development, UI/UX design, and artificial intelligence, I’m dedicated to building solutions that make a difference. Connect with me:

- **GitHub (Personal Contributions)**: [tabitha2005](https://github.com/tabitha2005)
- **Behance**: [your-behance](https://www.behance.net/your-behance)
- **Instagram**: [your-instagram](https://www.instagram.com/your-instagram)
- **LinkedIn**: [your-linkedin](https://www.linkedin.com/in/your-linkedin)

© 2025 Aluel's Investigation Swift by Aluel Tabitha. All rights reserved.
