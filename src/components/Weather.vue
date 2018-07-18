<template>
	<div class="container">
		<div class="row">
			<div class="col-md-6 mx-auto text-center bg-primary p-5 rounded">
				<h1 id="w-location" class="text-white">{{weatherDetail.location.name + ', ' + weatherDetail.location.region }}</h1>
				<h3 class="text-dark" id="w-desc">{{weatherDetail.current.condition.text}}</h3>
				<h3 id="w-string" class="text-white">{{weatherDetail.current.temp_c + ' °C'}}</h3>
				<img id="w-icon" :src=weatherDetail.current.condition.icon>
				<ul id="w-details" class="list-group mt-3 font-weight-bold">
					<li class="list-group-item" id="w-feels-like">Feels Like: {{weatherDetail.current.feelslike_c}} °C</li>
					<li class="list-group-item" id="w-wind">Wind: {{weatherDetail.current.wind_kph}} KM/H</li>
					<li class="list-group-item" id="w-cloud">Cloud: {{weatherDetail.current.cloud}} %</li>
					<li class="list-group-item" id="w-humidity">Relative Humidity: {{weatherDetail.current.humidity}} %</li>
				</ul>
				<hr>
				<button type="button" class="btn btn-success" data-toggle="modal" data-target="#locModal">Change Location</button>
			</div>
		</div>
		<!-- Modal -->
		<div class="modal fade" id="locModal" tabindex="-1" role="dialog" aria-labelledby="locModalLabel" aria-hidden="true">
			<div class="modal-dialog" role="document">
				<div class="modal-content">
					<div class="modal-header">
						<h5 class="modal-title" id="locModalLabel">Choose Location</h5>
						<button type="button" class="close" data-dismiss="modal" aria-label="Close">
							<span aria-hidden="true">&times;</span>
						</button>
					</div>
					<div class="modal-body">
						<form id="w-form">
							<div class="form-group">
								<label for="city">City</label>
								<input type="text" id="city" class="form-control" v-model="city" placeholder="Enter City Name">
							</div>
						</form>
					</div>
					<div class="modal-footer">
						<button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
						<button id="w-change-btn" type="button" class="btn btn-primary" @click="changeLocation">Save changes</button>
					</div>
				</div>
			</div>
		</div>
		<!-- Modal -->
	</div>
</template>

<script>
	export default {
		name: 'HelloWorld',
		data () {
			return {
				apikey :'7d35709828a7454eb7490932181707',
				city :  '',
				weatherDetail: {
					location:{
						name:'',
						region:''
					},
					current:{
						condition:{
							text:'',
							icon:''
						},
						temp_c: '',
						feelslike_c: '',
						wind_kphL:'',
						cloud:'',
						humidity:''
					}
				}
			}
		},
		created(){
			this.getWeather()
		},
		methods: {
			// Fetch weather from API
			getWeather: function(){
				this.getLocationData()
				this.$http.get(`http://api.apixu.com/v1/current.json?key=${this.apikey}&q=${this.city}`)
					.then(response => response.json())				
					.then(json => {
						this.weatherDetail = json;
						// console.log(this.weatherDetail)
					})
					.catch(error => {
						console.log("Error..." + error.response)
					});
					this.city = '';
			},
			changeLocation() {
				// Set location in local Storage
				this.setLocationData(this.city);
				
				// Get new inquired weather
				this.getWeather()

				//Close Modal
				$('#locModal').modal('hide');
			},
			// Set location in local Storage
			setLocationData(city) {
				localStorage.setItem('city', city);
			},
			// Get location from local Storage
			getLocationData() {
				if(localStorage.getItem('city') === null) {
					this.city = 'Kolkata';
				} else {
					this.city = localStorage.getItem('city');
				}
				return {
					city: this.city
				}
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	li:after{
	font-family: "Font Awesome 5 Free";
	-webkit-font-smoothing: antialiased;
	font-style: normal;
	font-variant: normal;
	text-rendering: auto;
	line-height: 1.5;
	font-weight: 900;
	float: right;
	font-size: 20px;
	}
	li:nth-child(1):after{
	content: "\f491";
	color: #fb891d;
	}
	li:nth-child(2):after{
	content: "\f56b";
	color: #20a215;
	}
	li:nth-child(3):after{
	content: "\f0c2";
	color: #2934ff;
	}
	li:nth-child(4):after{
	content: "\f043";
	color: #198fff;
	}
	@media screen and (max-width:768px){
		.col-md-6{
			height: 100vh;
			border-radius: 0% !important;
		}
	}
</style>
