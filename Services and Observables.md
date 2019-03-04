# Angular Recap
#### Services and Observables
##### Mar 04, 2019 | SRMKZILLA

 1. Create services folder in _src/app/_
	 ```console
	mkdir services
	 ```

> As an alternative, you can also create services folder using File Explorer.
 2. Change directory to _services_ in terminal
	 ```console
	cd services
	```
 3. Create a service. Let us name the service _auth_
	 ```console
	ng g service auth
	```
 4. Modify _app.module.ts_
> Unlike components, Angular CLI does not automatically add a service to _app.module.ts_. We will have to add this service manually in _app.module.ts_.

> Import the service and add it to the providers array in _app.module.ts_
```typescript
	import { AuthService } from './services/auth.service'
	...
	...
	...
	providers: [AuthService]
```

 5. Add _HttpClientModule_ to _app.module.ts_
 > We will be consuming RESTful APIs later in this application. Since we are here, let us import the _HttpClientModule_ anyway.

 > Import _HttpClientModule_ and add it to the imports array in _app.module.ts_
```typescript
	import { HttpClientModule } from '@angular/common/http'
	...
	...
	...
	imports: [HttpClientModule]
```
 6. Modify _auth.service.ts_
 > We will be consuming an online API to fetch the weather information of a city.

> **Steps**
> • Import _HttpClient_ and _map_
> • Create a private instance _http_ of _HttpClient_ in the constructor
> • Create a function to fetch weather details

>Refer to the code below
 ```typescript
import { Injectable } from '@angular/core';
// 1
import { HttpClient } from '@angular/common/http';
import { map } from 'rxjs/operators'

@Injectable({
	providedIn: 'root'
})

export class AuthService {
	// 2
	constructor(private http: HttpClient) { }
	
	// 3
	fetchWeather(city){
			const URL : string = "http://api.apixu.com/v1/current.json?key=256ee0e10b364c308ca125326190403&q="
			return this.http.get(URL+city).pipe(map(res => res));
	}
}
```
> The _fetchWeather(city)_ function returns an observable. An observable can be subscribed to execute a block of code after the data is received from the HTTP request.
 7. Let us create a component and use our newly created function to fetch data from the weather API
 >Navigate your terminal to the _components_ folder and create a new component, say _panel_

```console
ng g component panel
```
 8. Use _panel_ component in _app.component.html_
 >Replace the content of _app.component.html_ with the below code
```html
<app-panel></app-panel>
```
9. Modify _panel.component.html_
 >Replace the content of _panel.component.html_ with the below code
```html
<button (click)="buttonClicked()">Get weather</button>
<p *ngIf="weather != null">
The weather is {{weather}} °C!	
</p>
```
10. Modify _panel.component.ts_
 >Replace the content of _panel.component.ts_ with the below code
```typescript
import { Component, OnInit } from '@angular/core';
import { AuthService } from 'src/app/services/auth.service';

@Component({
	selector: 'app-panel',
	templateUrl: './panel.component.html',
	styleUrls: ['./panel.component.css']
})

export class PanelComponent implements OnInit {
	weather: any
	constructor(private authService: AuthService) { }

	ngOnInit() {

	}

	buttonClicked(){
		this.authService.fetchWeather("London").subscribe( (data: any) => {
			console.log(data)
			this.weather = data.current.temp_c
		})
	}
}
```
> __Notes__

> • _this.authService.fetchWeather("London")_ returns an observable that can be subscribed

> • We subscribe to this observable. The subscription code block is called when the HTTP request returns data

> • The response data can be accessed using the variable _data_

----

**Save and run! :)**
