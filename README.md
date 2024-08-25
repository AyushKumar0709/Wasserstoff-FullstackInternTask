To Run this project follow these setps:
* Create a folder name with "weather app".
*  Now create "index.html" file and copy the code.
*  Now create "custom.css" file and copy the code.
*  Now create "app.js" file and copy the code. 
*  Please carefully check the link tags in "index.html" is it properly linlking the file or not.
*  Now open the folder on vs code and open the "index.html" file in live server.
*  Thannk You for your time and consideration. 















Weather App Documentation
Overview
This weather app allows users to search for weather updates for a specific city. It displays current weather information, including temperature, pressure, humidity, and wind speed. Additionally, it shows a 5-day weather forecast using a swiper component for user-friendly navigation.

Key Components
1.Swiper Initialization

* Initializes a Swiper instance for displaying weather forecast data in a carousel format.
                            "var swiper = new Swiper(".mySwiper", {
                                slidesPerView: 3,
                                spaceBetween: 15,
                                freeMode: true,
                            });"
2.Element Selectors

* Selects DOM elements needed for interaction and display.
                              "const searchBtn = document.querySelector('.searchBtn');
                              const cityNameInput = document.querySelector('.citySearch');
                              const apiKey = 'b8e6343adb114d2c88af0939f6b1a6c4';
                              const weeks = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
                              const WeatherDegree = document.querySelector('.weatherInput');
                              const locationData = document.querySelector('.location-data');
                              const weatherPallete = document.querySelector('.weather-pallete');
                              const weatherUL = document.querySelector('.weather-ul');
                              const gettingStartedBtn = document.querySelector('.getting-started-btn');"

3.Event Listeners

* gettingStartedBtn: Adds active classes to the front page and weather container to display the weather section.
                                  "gettingStartedBtn.addEventListener('click', function () {
                                    document.querySelector('.frontpage').classList.add('active');
                                    document.querySelector('.weather-container').classList.add('active');
                                });"

* searchBtn: Fetches weather data based on user input city name and updates the display.
                                "searchBtn.addEventListener('click', function () {
                                  const cityName = cityNameInput.value.trim();
                                  if (cityName == "") {
                                      alert("Please Enter the City Name");
                                      return;
                                  } else {
                                      const geocoding_api_url = `http://api.openweathermap.org/geo/1.0/direct?q=${cityName}&limit=1&appid=${apiKey}`;
                                      fetch(geocoding_api_url)
                                          .then(res => res.json())
                                          .then(data => {
                                              if (!data.length) {
                                                  return alert(`${cityName} isn't a valid city Name`);
                                              } else {
                                                  const { name, lat, lon } = data[0];
                                                  gettingWeatherDetails(name, lat, lon);
                                              }
                                          })
                                          .catch(() => {
                                              alert("Error Occurred While Fetching the Coordinates");
                                          });
                                  }
                              });"
4. Function: createWeatherCard

* Generates HTML for weather cards to be displayed on the page. Different formats are used for the current weather and the forecast.
                                                      "function createWeatherCard(cityName2, weatherItem2, index2) {
                                                        if (index2 === 0) {
                                                            return `
                                                            <div class="location-data">
                                                                <div class="location-content">
                                                                    <i class='bx bxs-map'></i>
                                                                    <h3>${cityName2}</h3>
                                                                </div>
                                                                <p>${weatherItem2.dt_txt.split(' ')[0]}</p>
                                                            </div>
                                                            <div class="weather-Degree">
                                                                <h1>${(weatherItem2.main.temp - 273.15).toFixed(2)}°</h1>
                                                                <img src="https://openweathermap.org/img/wn/${weatherItem2.weather[0].icon}@4x.png" alt="weather-Degree-image" class="weather-Degree-img">
                                                                <h2>${weatherItem2.weather[0].description}</h2>
                                                            </div>
                                                            <div class="weather-pallete">
                                                                <div class="pallete-data">
                                                                    <i class='bx bx-water'></i>
                                                                    <small>${weatherItem2.main.pressure} M/B</small>
                                                                    <span>Pressure</span>
                                                                </div>
                                                                <div class="pallete-data">
                                                                    <i class='bx bxs-droplet-half' ></i>
                                                                    <small>${weatherItem2.main.humidity}%</small>
                                                                    <span>Humidity</span>
                                                                </div>
                                                                <div class="pallete-data">
                                                                    <i class='bx bx-wind' ></i>
                                                                    <small>${weatherItem2.wind.speed} M/S</small>
                                                                    <span>Wind Speed</span>
                                                                </div>
                                                            </div>
                                                            `;
                                                        } else {
                                                            return `
                                                            <li class="swiper-slide weather-data">
                                                                <span>${weatherItem2.dt_txt.split(' ')[0]}</span>
                                                                <img src="http://openweathermap.org/img/wn/${weatherItem2.weather[0].icon}@2x.png" alt="forecast-image" class="forecast-img">
                                                                <div class="icon-overlay"></div>
                                                                <small>${(weatherItem2.main.temp - 273.15).toFixed(2)}°</small>
                                                            </li>
                                                            `;
                                                        }
                                                    }"
5. Function: gettingWeatherDetails

* Fetches the 5-day weather forecast for a given city based on latitude and longitude. Processes the forecast data to generate HTML and updates the page.
                                                  "function gettingWeatherDetails(cityWeather, lat, lon) {
                                                    const weather_api_url = `http://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${apiKey}`;
                                                
                                                    fetch(weather_api_url)
                                                        .then(res => res.json())
                                                        .then(data => {
                                                            const forecastDays = [];
                                                            const fiveDaysForecast = data.list.filter(function (forecast) {
                                                                const forecastdate = new Date(forecast.dt_txt).getDate();
                                                                if (!forecastDays.includes(forecastdate)) {
                                                                    return forecastDays.push(forecastdate);
                                                                }
                                                            });
                                                
                                                            cityNameInput.value = "";
                                                            WeatherDegree.innerHTML = "";
                                                            locationData.innerHTML = "";
                                                            weatherPallete.innerHTML = "";
                                                            weatherUL.innerHTML = "";
                                                
                                                            fiveDaysForecast.forEach(function (weatherItem, index) {
                                                                if (index === 0) {
                                                                    WeatherDegree.insertAdjacentHTML('beforeend', createWeatherCard(cityWeather, weatherItem, index));
                                                                } else {
                                                                    weatherUL.insertAdjacentHTML('beforeend', createWeatherCard(cityWeather, weatherItem, index));
                                                                }
                                                            });
                                                        })
                                                        .catch(() => {
                                                            alert('Error Occurred While Fetching the Coordinates of Weather');
                                                        });
                                                }"

Notes
* API Key: Ensure to replace the apiKey with your own API key from OpenWeatherMap.
* CORS: If facing CORS issues, you might need to set up a proxy or use a different API endpoint.
