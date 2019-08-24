# Stub

## Sample Usage

### Class testing a component

- Classes, `Object`'s, `object`'s, and `{ object }`'s

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

### Testing a service

 - Http calls
 - Window Storage, Navigator

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
});
```