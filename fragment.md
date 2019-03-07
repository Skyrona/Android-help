# Cycle de vie d'un fragment

Pour créer un fragment, on crée une sous-classe de **Fragment** (ou une sous-classe existante).

La classe **Fragment** contient plusieurs méthodes de rappel (callback).

De base, il faudrait au moins implémenter les méthodes suivantes : 

**onCreate** : le système l'appelle lors de la création du fragment. Il faudrait initialser les composants essentiels du fragment que l'on souhaite conserver lors que la pause ou de l'arrêt.

**onCreateView()** : le système l'appelle lors de la création de l'interface utilisateur pour la 1ère fois. Pour dessiner une UI pour le fragment, on doit renvoyer une **View** qui est la racine du layout de notre fragment. On peut renvoyer **null** si le fragment ne fournit pas d'UI.

**onPause()** : le système l'appelle lorsque l'utilisateur quitte le fragment. On enregistre les changements que l'on veut conserver pour les sessions suivantes (l'utilisateur pourrait ne pas revenir).

## Etat d'un fragment

Un fragment (comme une activité) peut exister sous trois états : 
* Resumed : le fragment est visible dans l'activité en cours d'exécution
* Paused : une autre activité a le focus mais l'activité contenant notre fragment est toujours visible
* Stopped : le fragment n'est plus visible, un fragment arrêté est toujours vivant mais il n'est plus visible pour l'utilisateur et sera tué si l'activité est tué.

On peut préserver l'état de l'UI d'un fragment à travers les changements et les morts avec une combinaison de **onSaveInstanceState(Bundle)**, de **ViewModel** et d'un local storage persistent.

La principale différence dans le cycle de vie d'une activité et d'un fragment réside dans la façon dont il est enregistré dans son **back stack**.
Une activité est enregistré par défaut dans son **back stack** tandis qu'un fragment est placé dans un **back stack** uniquement si l'instance est sauvé en appellant **addToBackStack()** pendant la transaction qui retire le fragment.

## Coordination avec le cycle de vie de l'activité

Le cycle de vie de l'activité affecte celui du fragment, par exemple lorsque l'activité reçoit **onPause()**, chaque fragment dans celle-ci reçoit également **onPause()**.

Les fragments ont des callbacks de cycle de vie supplémentaires qui gère l'interaction avec l'activité : 
* **onAttach()** : appellé lorsque le fragment est associé avec l'activité (l'activité est passée içi)
* **onCreateView** : appellé lorsque la view est associée avec le fragment
* **onActivityCreated()** : appellé lorsque le **onCreate()** de l'activité est retourné
* **onDestroyView()** : appellé lorsque la view associée avec le fragment est retiré
* **onDetach()** : appellé lorsque le fragment est dissocié de l'activité