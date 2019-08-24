# Stub

Stub automates the creation of object doubles and their necessary spies and mocks, while significantly reducing boilerplate. The tool supports:
 - Classes and objects
 - HttpClient calls
 - Window Objects (Storage, Navigator)
 - Many Angular Material Components
 - Many more

## Sample Usage

### Classes and objects

```
import { Stub } from '@jazz/stub';

describe('RestaurantSupplyManagerComponent Class', () => {
  let component: RestaurantSupplyManagerComponent;

  // Stubs
  let restaurantService: Stub<RestaurantService>

  beforeEach(async(() => {
      restaurantService = new Stub(RestaurantService);
      
      TestBed.configureTestingModule({
        providers: [
          RestaurantSupplyManagerComponent,
          { provide: RestaurantStockService, useValue: restaurantStockServiceStub }
        ],
      });
  }));

  beforeEach(() => {
    component = TestBed.get(RestaurantSupplyManagerComponent);
  });

  it('onSeeChimichurriStock should request the number of chimichurri bottles left on stock', () => {
    // Arrange

    // Act
    component.onSeeChimichurriStock();
    
    // Assert
    expect(component.restaurantStockService.getChimichurriStock).toHaveBeenCalledTimes(1);
  });

  it('onOrderMoreChimichurri should order more chimichurri bottles', () => {
    // Arrange
    const numberOfBottles = Number.MAX_VALUE;

    // Act
    component.onOrderMoreChimichurri(numberOfBottles);
    
    // Assert
    expect(component.restaurantStockService.createChimichurriOrder).toHaveBeenCalledWith(numberOfBottles);
  });
})
```

### Http calls, Browser Window (Storage, Navigator)


```
import { HttpClient } from '@angular/common/http';
import { TestBed, async } from '@angular/core/testing';
import { of as observableOf} from 'rxjs';

import { Stub } from '@jazz/stub';

describe('RestaurantStockService', () => {
  let service: RestaurantStockService;

  let httpClientStub: Stub<HttpClient>;

  beforeEach(async(() => {
    httpClientStub = new Stub(HttpClient);

    TestBed.configureTestingModule({
      providers: [{ provide: HttpClient, useValue: httpClientStub }],
    });
  }));

  beforeEach(() => {
    service = TestBed.get(SearchAnalyticsService);
  });

  // Http calls
  it('createChimichurriOrder should issue a POST request to the Restaurant API', () => {
    // Arrange
    const numberOfBottles: 2;
    const expectedUrl = 'url';
    const expectedBody = numberOfBottles;
    const expectedOptions = null;

    // Act
    service.createChimichurriOrder(numberOfBottles);

    // Arrange
    expect(service.httpClient.post).toHaveBeenCalledWith(expectedUrl, expectedBody, expectedOptions);
  });

  // Window Storage
  it(getUserPreferences should retrieve the user-preferences from session storage,() => {
    // Arrange
    const preferencesMock = { language: 'en-UK', secondLanguage: 'es-US' };
    const sessionStorageStub: Stub<WindowProperty.SessionStorage> = new Stub(WindowProperty.SessionStorage, preferencesMock);
    // you may also set items in session storage this way:
    sessionStorageStub.setItem('item1', 'item1Value');

    // Act
    service.getUserPreferences(numberOfBottles);  // lets assume this methods retrieves the preferences from sessionStorage;

    // Arrange
    expect(window.sessionStorage.getItem('language')).toEqual('en-UK');
    expect(window.sessionStorage.getItem('secondLanguage')).toEqual('es-US');
    expect(window.sessionStorage.getItem('item1')).toEqual('item1Value');
  })

  // Window Navigator
  it(getUserPreferences should retrieve the user-preferences from session storage,() => {
    // Arrange
    const navigatorValuesMock = { locale: 'en-CA' };
    const navigatorStub: Stub<WindowProperty.Navigator> = new Stub(WindowProperty.Navigator, navigatorValuesMock);

    // Act
    service.getUserPreferences(numberOfBottles);  // lets assume this methods retrieves the preferences from sessionStorage;

    // Arrange
    expect(window.localStorage.getItem('language')).toEqual('en-CA');
  })
});
```
