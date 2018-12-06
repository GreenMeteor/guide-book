# Common Setup
By default every HumHub has a `common.php` file, which can be found in `/protected/config/common.php`.
This guide will show you a number of snippets that can be used within your HumHub's `common.php` file.

### URL Rewriting
As descripted in the official [Docs](http://docs.humhub.org/admin-installation-configuration.html#url-rewriting-optional), this is why you make your URL fancy and look better for search engines.
```php
<?php

return [
    'components' => [
        'urlManager' => [
            'showScriptName' => false,
            'enablePrettyUrl' => true,
        ],
    ]
];
```

### UrlManager rules & PrettyUrl activated

```php
<?php

return [
    'components' => [
        'urlManager' => [
            'enablePrettyUrl' => true,
            'showScriptName' => false,
            'enableStrictParsing' => false,
            'rules' => [
                'posts' => 'post/index',
                'post/<id:\d+>' => 'post/view',
]
        ],
    ],
];
```

### Stream Suppressing
The following is how to suppress entries like the ones from cfile & gallery modules.
```php
<?php

return [
    'modules' => [
        'stream' => [
            'streamSuppressQueryIgnore' => [
                'humhub\modules\cfiles\models\File',
                'humhub\modules\gallery\models\Media'
            ]
        ]
    ]
]
```

### Default Permissions
Set the defule view permissions for the following.
```php
<?php

return [
    'params' => [
        'defaultPermissions' => [
            'humhub\modules\user\permissions\ViewAboutPage' => [
                \humhub\modules\user\models\User::USERGROUP_SELF => \humhub\libs\BasePermission::STATE_ALLOW,
                \humhub\modules\user\models\User::USERGROUP_USER => \humhub\libs\BasePermission::STATE_ALLOW,
                \humhub\modules\user\models\User::USERGROUP_FRIEND => \humhub\libs\BasePermission::STATE_ALLOW,
                \humhub\modules\user\models\User::USERGROUP_GUEST => \humhub\libs\BasePermission::STATE_ALLOW,
            ],
        ]
    ]
]
```

### Sort Members List

```php
<?php

return [
    'modules' => [
        'directory' => [
             'memberListSortField' => 'lastname'
        ]
    ]
];
```

### Database timezone change + PrettyUrl
Replace `TIME_ZONE` with your timezone (Example: `+09:00` or `-09:00`)
```php
<?php

return [
    'components' => [

        'db' => [
            'class' => 'yii\db\Connection',
            'on afterOpen' => function($event) {
            $event->sender->createCommand("SET time_zone = 'TIME_ZONE'")->execute();
        }
    ],
        'urlManager' => [
            'showScriptName' => false,
            'enablePrettyUrl' => true,
        ],
    ]
];
```

### Sync Queue
For small communities.
```php
<?php

return [
    'components' => [
        'queue' => [
            'class' => 'humhub\modules\queue\driver\Sync',
        ],
    ],
]
```
[Julian](https://community.humhub.com/u/buddha/) recommends running one asynchronous queue since it allows long running tasks to run in the background, in the future there will be more of such jobs.

### Social Logins & PrettyUrl activated
Social Logins + PrettyUrl in full compared to the official [docs](http://docs.humhub.org/admin-authentication.html).
```php
<?php

return [
    'components' => [
    // ...
        'authClientCollection' => [
            'clients' => [
                // ...
              'facebook' => [
                    'class' => 'humhub\modules\user\authclient\Facebook',
                    'clientId' => 'Your Facebook App ID here',
                    'clientSecret' => 'Your Facebook App Secret here',
               ],
              'github' => [
                    'class' => 'humhub\modules\user\authclient\GitHub',
                    'clientId' => 'Your GitHub Client ID here',
                    'clientSecret' => 'Your GitHub Client Secret here',
                    // require read access to the users email
                    // https://developer.github.com/v3/oauth/#scopes
                    'scope' => 'user:email',
               ],
              'google' => [
                    'class' => 'humhub\modules\user\authclient\Google',
                    'clientId' => 'Your Client ID here',
                    'clientSecret' => 'Your Client Secret here',
               ],
              'live' => [
                    'class' => 'humhub\modules\user\authclient\Live',
                    'clientId' => 'Your Microsoft application ID here',
                    'clientSecret' => 'Your Microsoft application password here',
                ],
              'twitter' => [
                'class' => 'humhub\modules\user\authclient\Twitter',
                   'attributeParams' => [
                       'include_email' => 'true'
                   ],
                    'clientId' => 'Your Twitter client key here',
                    'clientSecret' => 'Your Twitter client secret here',
                ],
              'linkedin' => [
                    'class' => 'humhub\modules\user\authclient\Linkedin',
                    'clientId' => 'Your Microsoft application ID here',
                    'clientSecret' => 'Your Microsoft application password here',
                ],
              ],
             ],
       //...
        'urlManager' => [
            'showScriptName' => false,
            'enablePrettyUrl' => true,
        ],
    ]
];
```

### Email streamOptions
```php
<?php

return [
  'components' => [
    'mailer' => [
      'transport' => [
        'streamOptions' => [ 
          'ssl' => [ 
            'allow_self_signed' => true,
            'verify_peer' => false,
            'verify_peer_name' => false,
          ]
        ]
      ]
    ]
  ]
];
```
