document.addEventListener('deviceready', onDeviceReady, false);

function onDeviceReady() {
    // Check if dark mode is enabled
    const darkModeMediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
    
    // Update splash screen background color based on phone mode
    const splashScreen = document.getElementById('splash-screen');

    if (darkModeMediaQuery.matches) {
        // Dark mode is enabled
        splashScreen.style.backgroundColor = '#000000'; // Set dark mode background color
    } else {
        // Light mode is enabled
        splashScreen.style.backgroundColor = '#ffffff'; // Set light mode background color
    }
}
