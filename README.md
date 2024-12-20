# Interactivemap
# Map Testing on interactive Lizmap Cambodia accessmod
lizMap.events.on({
    'uicreated': () => {
        // Function to wait for an element in the DOM
        function waitForElm(selector) {
            return new Promise(resolve => {
                if (document.querySelector(selector)) {
                    return resolve(document.querySelector(selector));
                }

                const observer = new MutationObserver(mutations => {
                    if (document.querySelector(selector)) {
                        resolve(document.querySelector(selector));
                        observer.disconnect();
                    }
                });

                observer.observe(document.body, {
                    childList: true,
                    subtree: true
                });
            });
        }

        // Automatically activate a key feature and hide certain buttons
        waitForElm('#button-Mapillary').then((elm) => {
            elm.click(); // Simulate a click to activate Mapillary
            $('#button-switcher').hide(); // Hide the map switcher button
        });

        // Add a search bar to the UI
        const searchContainer = document.createElement('div');
        searchContainer.id = 'hospital-search-container';
        searchContainer.style.position = 'absolute';
        searchContainer.style.top = '10px';
        searchContainer.style.left = '10px';
        searchContainer.style.zIndex = '1000';
        searchContainer.style.backgroundColor = '#fff';
        searchContainer.style.padding = '10px';
        searchContainer.style.borderRadius = '5px';
        searchContainer.style.boxShadow = '0px 2px 5px rgba(0, 0, 0, 0.2)';
        searchContainer.innerHTML = `
            <input type="text" id="hospital-search-bar" placeholder="Search for a hospital..." style="width: 200px; padding: 5px;">
            <button id="hospital-search-button" style="padding: 5px 10px; margin-left: 5px;">Search</button>
            <div id="hospital-search-results" style="margin-top: 10px;"></div>
        `;
        document.body.appendChild(searchContainer);

        // Data for the search functionality
        const hospitals = {
            'phnom penh hospital': {
                image: 'phnom_penh_hospital.jpg',
                description: 'Phnom Penh Hospital: A state-of-the-art medical facility located in the capital city.'
            },
            'siem reap hospital': {
                image: 'siem_reap_hospital.jpg',
                description: 'Siem Reap Hospital: Known for its excellent care and proximity to Angkor Wat.'
            },
            'battambang hospital': {
                image: 'battambang_hospital.jpg',
                description: 'Battambang Hospital: Providing quality healthcare services to the western region.'
            }
        };

        // Search functionality
        document.getElementById('hospital-search-button').addEventListener('click', () => {
            const query = document.getElementById('hospital-search-bar').value.toLowerCase();
            const resultsContainer = document.getElementById('hospital-search-results');
            resultsContainer.innerHTML = '';

            if (hospitals[query]) {
                const hospital = hospitals[query];
                resultsContainer.innerHTML = `
                    <img src="${hospital.image}" alt="${query}" style="width: 100%; border-radius: 5px; margin-bottom: 5px;">
                    <p>${hospital.description}</p>
                `;
            } else {
                resultsContainer.innerHTML = '<p>No results found. Please try another search.</p>';
            }
        });
    }
});

