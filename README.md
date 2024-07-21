# PWA-Text-Editor
## Progressive Web Applications (PWA) Challenge: Text Editor


This project is a Progressive Web Application (PWA) text editor that allows users to create and store notes or code snippets with or without an internet connection. It utilizes IndexedDB for data persistence and can be installed as a standalone application.

# Table of Contents
- Installation
- Usage
- Features
- How It Works
- Technologies Used

# Installation
- Clone the Repository:

        git clone https://github.com/Rualexandra/PWA-Text-Editor.git
        cd PWA-Text-Editor

- Install Dependencies:
Navigate to the client directory and install the necessary dependencies:

        cd client
        npm install
- Build the Application:

        npm run build

- Start the Application:

    npm start
    The application will be available at http://localhost:8080 (or another port if configured).

# Usage
- Open the Text Editor:

        Navigate to http://localhost:8080 in your browser.

- Create and Edit Notes:
Use the text editor to create and edit notes or code snippets.

- Data Persistence:
Data is saved automatically to IndexedDB when the editor loses focus.

- Install the PWA:
Click the install button in the browser's address bar to install the application as a standalone PWA.

# Features
Offline functionality
IndexedDB for data persistence
Service worker for caching static assets
PWA installable on desktop and mobile devices

# How It Works
## IndexedDB
The application uses IndexedDB, a low-level API for storing large amounts of structured data. The idb library, a lightweight wrapper around IndexedDB, is used to simplify database operations.

## Initialization:
The database is initialized with a store named jate (Just Another Text Editor).

        const initdb = async () =>
        openDB('jate', 1, {
            upgrade(db) {
            if (db.objectStoreNames.contains('jate')) {
                console.log('jate database already exists');
                return;
            }
            db.createObjectStore('jate', { keyPath: 'id', autoIncrement: true });
            console.log('jate database created');
            },
        });
        
## Saving Data:
The content of the editor is saved to IndexedDB whenever the editor loses focus.

        this.editor.on('blur', () => {
        putDb(localStorage.getItem('content'));
        });

## Loading Data:
The content is loaded from IndexedDB (or localStorage as a fallback) when the application starts.

        getDb().then((data) => {
        console.info('Loaded data from IndexedDB, injecting into editor');
        this.editor.setValue(data || localData || header);
        });

## Service Worker
The application registers a service worker to cache static assets and enable offline functionality. The service worker is generated using Workbox.

## Registering the Service Worker:

        if ('serviceWorker' in navigator) {
        const workboxSW = new Workbox('/src-sw.js');
        workboxSW.register();
        } else {
        console.error('Service workers are not supported in this browser.');
        }

## Caching Assets:

The service worker caches static assets during the installation phase to ensure the application works offline.

        self.addEventListener('install', (event) => {
        event.waitUntil(
            caches.open('static-cache-v1').then((cache) => {
            return cache.addAll(['/', '/index.html', '/bundle.js']);
            })
        );
        });
## PWA Manifest
The Webpack PWA Manifest plugin is used to generate the manifest.json file, which provides metadata for the PWA.

        new WebpackPwaManifest({
        name: 'Text Editor',
        short_name: 'TextEdit',
        description: 'A simple text editor PWA',
        background_color: '#ffffff',
        theme_color: '#317EFB',
        start_url: '.',
        display: 'standalone',
        icons: [
            {
            src: path.resolve('src/images/icon.png'),
            sizes: [96, 128, 192, 256, 384, 512],
            },
        ],
        }),
## Technologies Used
    JavaScript
    Webpack
    Babel
    Workbox
    IndexedDB
    idb Library
    Service Workers
    PWA Manifest

# Reach me at:
https://github.com/Rualexandra