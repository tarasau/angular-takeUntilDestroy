# Angular - takeUntilDestroy
Create custom ngOnDestroy life cycle hook

`import { Subject } from 'rxjs';

export function TakeUntilDestroy(constructor: any) {
  const originalNgOnDestroy = constructor.prototype.ngOnDestroy;

  constructor.prototype.componentDestroy = function () {
    this._takeUntilDestroy$ = this._takeUntilDestroy$ || new Subject();
    return this._takeUntilDestroy$.asObservable();
  };

  constructor.prototype.ngOnDestroy = function () {
    if (this._takeUntilDestroy$) {
      this._takeUntilDestroy$.next(true);
      this._takeUntilDestroy$.complete();
    }
    if (originalNgOnDestroy && typeof originalNgOnDestroy === 'function') {
      originalNgOnDestroy.apply(this, arguments);
    }
  };
}`

Usage example:

`@Component({
  selector: 'example-component',
})
@TakeUntilDestroy
export class ExampleComponent implements OnInit, OnDestroy {
  componentDestroy;

  constructor() {}
  
  ngOnInit() {}

  ngOnDestroy() {
    this.store.dispatch(new ExampleAction(''));
  }
  
  onKeywordChange(keyword: string) {
    this.store.dispatch(new ExampleAction(keyword));
  }
}`
