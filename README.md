## Versionamiento

#### Mensaje

Cada mensaje de un commit debe tener la siguiente formula:

:emoji: + [project] + message

Ejemplos: 


- :white_check_mark: [Dashboard] add tests to AuthModule
- :construction: [Backend] work in events api.
- :heavy_plus_sign: [Graphql] add nodemon.

Emojis:

- :art: Improving structure / format of the code.
- :zap: Improving performance.
- :fire: Removing code or files.
- :bug: Fixing a bug.
- :ambulance: Critical hotfix.
- :memo: Writing docs.
- :lipstick: Updating the UI and style files.
- :white_check_mark: Adding tests.
- :lock: Fixing security issues.
- :rotating_light: Removing linter warnings.
- :construction: Work in progress.
- :green_heart: Fixing CI Build.
- :arrow_down: Downgrading dependencies.
- :arrow_up: Upgrading dependencies.
- :pushpin: Pinning dependencies to specific versions.
- :construction_worker: Adding CI build system.
- :recycle: Refactoring code.
- :whale: Work about Docker.
- :heavy_plus_sign: Adding a dependency.
- :heavy_minus_sign: Removing a dependency.
- :wrench: Changing configuration files.
- :globe_with_meridians: Internationalization and localization.
- :twisted_rightwards_arrows: Merging branches.
- :loud_sound: Adding logs.
- :mute: Removing logs.
- :building_construction: Making architectural changes.
- :pencil2: Fixing typos.

Los emojis estan basados en [https://gitmoji.carloscuesta.me/](https://gitmoji.carloscuesta.me/){:target="_blank"}, se recomienda el uso de [*gitmoji-cli*](https://github.com/carloscuesta/gitmoji-cli){:target="_blank"} para usar desde terminal mucho más fácil.

Ejemplo:

![gitmoji-cli](https://cloud.githubusercontent.com/assets/7629661/20454643/11eb9e40-ae47-11e6-90db-a1ad8a87b495.gif)


> NOTA: La referencia de *gitmoji* es util pero muy larga, SOLO se deben usar los emojis anteriormente mostrados. Si se cree que debe agregar un nuevo emoji por favor enviar un PR explicando la razón.


#### Ammend

Si se realiza un commit con una parte de los cambios y se necesita adjuntar una segunda parte, es recomendable fusionar estos dos o sobrescribir el commit así:

*Aquí se agregan los primeros cambios*

```
git add .
git commit -m "primer commit"
```

*Aquí se realizan los cambios siguientes*

```
git add .
git commit --amend -m "Commit informando los nuevos cambios"
```

Si el commit ya fué enviado "*push*" se debe hacer nuevamente "*push -f*" para sobrescribir el commit viejo , de lo contrario se hace el push request o merge request normal.

> *NOTA:* Esto solamente sobrescribe el commit anterior, para fusionar más de 2 commits ver el **Rebase** a continuación

#### Rebase

Si hay más de un commit:
[usa `git rebase` interactively](https://help.github.com/articles/about-git-rebase/)
y se quiere integrar en un solo commit más completo y bien explicado:

```
git rebase -i <commit-id>
```

*<commit-id> es el commit hasta donde se quiere unificar.*
*Agrega el "~1" para incluir también el commit hasta donde se quiere unificar.*
    
*Va a mostrar el listado de commits del mas antiguo al mas nuevo*
```
pick e162eb3 :soccer: Commit 3
pick 1b9a12d :soccer: Commit 2
pick 7b4d76e :soccer: Commit 1
```

*Y se debería cambiar así (en caso de querer unificarlos todos):*
```
pick e162eb3 :soccer: Commit 3
s 1b9a12d :soccer: Commit 2
s 7b4d76e :soccer: Commit 1
```
*luego de unificar los commits, debes poner un solo mensaje:*
```
# This is a combination of 3 commits.
# This is the 1st commit message:

:soccer: Commit 3

# This is the commit message #2:

:soccer: Commit 2

# This is the commit message #3:

:soccer: Commit 1
```
*Así:*
```
# This is a combination of 3 commits.
# This is the 1st commit message:

:soccer: Commit 1 contains commit 2 and commit 3
```

## Typescript/Angular

Para projectos Angular se sigue la guía oficial de [*Angular Style Guide*](https://angular.io/guide/styleguide){:target="_blank"}


#### Atributos en objetos

Si el atributo dentro un objeto tiene el mismo nombre no hace falta agregar el nombre de nuevo.

> Bad example

```ts
const toSend = {
  'status': status
};
```

> Good example

```ts
const toSend = {
  status
};
```

#### Uso de short imports

Para evitar el rompimiento de links en módulos y evitar referencias al path se debe usar los `short imports`.

> Bad example

```ts
import { environment } from './../../environments/environment';
import { MyService } from './../services/myservice/myservice';
```

> Good example

```ts
import { environment } from '@environments/environment';
import { MyService } from '@module/services/myservice/myservice';
```

El uso de short imports se explica en este [*video*](https://youtu.be/EkbozO6fxv4){:target="_blank"}

#### Uso de path en los requests

Para tener una mejor lectura y no generar lineas largas, se debe seperar el `path` en una variable.

> Bad example

```ts
return this.http.get<EventMessages[]>(`${this.apiUrl}/api/v1/events/${eventId}/history/`);
```

> God example

```ts
const path = `${this.apiUrl}/api/v1/events/${eventId}/history/`;
return this.http.get<EventMessages[]>(path);
```

## Ngrx/Angular

#### Organización de carpetas

Se sigue la estructura de *Angular Style Guide* pero sumando redux la estructura por módulo debe ser así:

![folder](https://imgur.com/wAEfxHQ)

#### Archivos Index

La regla anterior divide por responsabilidad cada artefacto de Angular, con el propósito de generar y adjuntas varios artefacto debe haber un `index.ts`, de esta manera:

```ts
import { EventEffects } from './event.effects';
import { MesageEffects } from './message.effects';
import { OperatorEffects } from './operator.effects';

export const EFFECTS: any[] = [
  EventEffects,
  MesageEffects,
  OperatorEffects
];
```

> Nota: La única excepción es el `index` de la carpeta `reducers` en donde se adjuntan todos los reducers de forma diferente.

> Nota: Se esta evaluando el uso de `provideIn: root` de Angular para habilitar Tree shaking en los servicios, lo que evitaria definir un index.ts para todo lo que sea `Injectable`. [Info](https://github.com/mgechev/angular-performance-checklist#tree-shakeable-providers){:target="_blank"}


#### Boilerplate de un módulo

Cada vez que se cree un nuevo módulo y para evitar gran y largo archivo de configuración por módulo se recomienda la siguiente estructura:

> Good example

```ts
import * as fromContainers from './containers';
import * as fromComponents from './components';
import * as fromEffects from './effects';
import * as fromReducers from './reducers';
import * as fromServices from './services';
import * as fromGuards from './guards';

@NgModule({
  imports: [
    CommonModule,
    ReactiveFormsModule,
    FormsModule,
    InboxRoutingModule,
    SharedModule,
    TranslateModule,
    StoreModule.forFeature('inbox-module', fromReducers.reducers, {
      metaReducers: fromReducers.metaReducers
    }),
    EffectsModule.forFeature(fromEffects.EFFECTS)
  ],
  declarations: [
    ...fromComponents.COMPONENTS,
    ...fromContainers.CONTAINERS,
  ],
  providers: [
    ...fromGuards.GUARDS,
    ...fromServices.SERVICES
  ]
})
export class InboxModule { }
```

#### Envio de actions

Para tener una mejor lectura y no generar lineas largas, se debe seperar el `action` en una variable constante.

> Bad example

```ts
this.store.dispatch(new LoadDashboardsRequest({filter}));
```

> Good example

```ts
const action = new LoadDashboardsRequest({filter});
this.store.dispatch(action);
```

#### Uso de map en Effects

Todas las acciones de redux tienen un `payload` por ende siempre se debe ejecutar un `map` para obtener el valor del `payload` antes de ser procesado por un `pipe`.

> Bad example

```ts
@Effect()
loadSomething$ = this.actions$
.pipe(
    ofType<ActionType>(SomethingActionTypes.LoadSomething),
    switchMap(action) => {
      const payload = action.payload;
      // code
    }),
);
```

> Good example

```ts
@Effect()
loadSomething$ = this.actions$
.pipe(
    ofType<ActionType>(SomethingActionTypes.LoadSomething),
    map(action => action.payload),
    switchMap(payload) => {
        // code
    }),
);
```

#### Uso de OfType en Effects

Para poder tipar el `action` se debe hacer uso del tipado de `ofType` y no hacerlo despues.

> Bad example

```ts
@Effect()
loadSomething$ = this.actions$
.pipe(
    ofType(SomethingActionTypes.LoadSomething),
    map((action: ActionType) => action.payload)
    switchMap(payload) => {
      // code
    }),
);
```

> Good example

```ts
@Effect()
loadSomething$ = this.actions$
.pipe(
    ofType<ActionType>(SomethingActionTypes.LoadSomething),
    map(action => action.payload)
    switchMap(payload) => {
      // code
    }),
);
```


## Testing/Angular

#### Alinear las inyecciones

Cuando se escriban pruebas unitarias y estas tengas inyección de dependencias, se debe alinear todas con `useValue`, asi es más fácil revisar las inyecciones de esa prueba. 

> Bad example

```ts
providers: [
    { provide: 'API_URL', useValue: environment.api },
    { provide: EventsService, useClass: EventsService },
    { provide: PouchDBService, useClass: PouchDBService },
    { provide: LoggerService, useClass: LoggerService }
]
```

> Good example

```ts
providers: [
    { provide: 'API_URL',      useValue: environment.api },
    { provide: EventsService,  useClass: EventsService },
    { provide: PouchDBService, useClass: PouchDBService },
    { provide: LoggerService,  useClass: LoggerService }
]
```

## Python - Flask

Pep8 para el cual usamos el linter de flake8
    https://www.python.org/dev/peps/pep-0008/

#### Variables

Cuando usemos variables que representen estados, valores, elementos en texto ejemplo:

> Bad example

```python
operator_status = {
    'status': 'active',
}

operator_status = {
    'max_quantity': 18,
    'gender': 'Masculino',
}

operator_status = {
    'city': 'Bogotá DC',
}
```

Se deben crear variables globales para que se maneje información homogénea en todo el proyecto
entonces debería verse así

```python
ACTIVE_CHOICES =  'active'
MAX_QUANTITY_CHOICES =  18
GENDER_MALE_CHOICES =  'Masculino'
CITY_BOGOTA_CHOICES =  'Bogotá DC'
```

Estas variables van alojadas en un data.py

> Good example

```python
from app.XX import data

operator_status = {
    'status': data.ACTIVE_CHOICES,
}

operator_status = {
    'max_quantity': data.MAX_QUANTITY_CHOICES,
    'gender': data.GENDER_MALE_CHOICES,
}

operator_status = {
    'city': data.CITY_BOGOTA_CHOICES,
}
```

#### Generalities

* Use comillas sencillas `''` para strings.

#### URLS

Usar `/` slash al final de las URLs

> Bad example

```python
view_path = 'events/_view/by_opened_by_operator'
```

> Good example

```python
view_path = 'events/_view/by_opened_by_operator/'
```

#### Importaciones

Usar una linea por cada dependencia, aún si son del mismo paquete.

> Bad example

```python
from app.method import function_one, function_two, function_tree
```

> Good example

```python
from app.method import function_one
from app.method import function_two
from app.method import function_tree
```

En el caso de ser demasiadas, debemos importar solamente el paquete base

> Good example

```python
from app import method

response = method.function_two()
response = method.function_tree()
...
```

#### Get context

Use asignación directa cuando pase una **sola** variable al contexto

> Bad example

```python
def get_context_data(self, **kwargs):
    ...

    context.update({
        'comment_form': CommentForm,
    })

    ...
```

> Good example

```python

def get_context_data(self, **kwargs):
    ...

    context['comment_form'] = CommentForm

    ...
```

Use `update` cuando pase multiples variables.

 > Bad example

 ```python

 def get_context_data(self, **kwargs):
     ...

     context['comment_form'] = CommentForm
     context['items_list'] = items_list
     context['users_list'] = users_list

     ...
 ```

 > Good example

 ```python
 def get_context_data(self, **kwargs):
     ...

     context.update({
         'comment_form': CommentForm,
         'items_list': items_list,
         'users_list': users_list,
     })

     ...
 ```


#### Llamados a función

Se deben especificar los valores en el llamado de una función

```python
def function_name(attr1, attr2):
    return True
```

> Bad example

```python
function_name(626, "Modelo 50"):
    return True
```

> Good example

```python
function_name(attr1=626, attr2="Modelo 50"):
    return True
```

#### Funciones después de 80 caracteres

Se debe partir la función en varias líneas

```python
def function_name(attr1, attr2):
    return True
```

> Bad examples

```python
# Has 93 characters
def function_name(attr_name_1=attr_data_1, attr_name_2=attr_data_2, attr_name_3=attr_data_3):
    return True
```

```python
def function_name(attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3
):
    return True
```

```python
def function_name(
    attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3):
    return True
```

> Good example

```python
def function_name(
    attr_name_1=attr_data_1,
    attr_name_2=attr_data_2,
    attr_name_3=attr_data_3
):
    return True
```


#### Diccionarios/Json después de 80 caracteres

> Bad example

```python
# Has 93 lines
dict_1 = {'first_parameter_1': 'first_parameter_1', 'first_parameter_2': 'first_parameter_2'}
```

```python
dict_1 = {
    'first_parameter_1': 'first_parameter_1',
    'first_parameter_2': 'first_parameter_2'}
```

> Good example

```python
dict_1 = {
    'first_parameter_1': 'first_parameter_1',
    'first_parameter_2': 'first_parameter_2'
}
```

#### String después de 80 caracteres

> Bad example

```python
# Has 93 lines
string_1 = 'Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, consectetur, ...'
```

> Good example

```python
string_1 = 'Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, '
    'consectetur, ...'
```

```python
string_1 = '''
    Neque porro quisquam est qui dolorem ipsum quia dolor si
    t amet, consecteturt amet, consecteturt amet, consecteturt amet,
    consectetu t amet, consectetur, ...
    '''
```

