# ITK Dev – User management test

This is a example/test/develop setup for [ITK Dev – User
management](https://github.com/itk-dev/user-management-bundle).

## Setup for development

Pull
[itk-dev/user-management-bundle](https://github.com/itk-dev/user-management-bundle)
into `packages/itk-dev/user-management-bundle`:

```sh
mkdir -p packages/itk-dev
git clone --branch=master https://github.com/itk-dev/user-management-bundle packages/itk-dev/user-management-bundle
```

## Start the show

```sh
docker-compose up --detach
docker-compose exec phpfpm composer config repositories.itk-dev/user-management-bundle path packages/itk-dev/user-management-bundle
docker-compose exec phpfpm composer require itk-dev/user-management-bundle:dev-master
docker-compose exec phpfpm composer install
docker-compose exec phpfpm bin/console doctrine:migrations:migrate --no-interaction
```

Login form: [http://0.0.0.0:8880/login](http://0.0.0.0:8880/login)

## Create user and send notifications

Create a new user:

```sh
docker-compose exec phpfpm bin/console fos:user:create test@example.com test@example.com password
```

Notify the user via email:

```sh
docker-compose exec phpfpm bin/console itk-dev:user-management:notify-users-created test@example.com
```

Check the email sent to the user on [http://0.0.0.0:8885](http://0.0.0.0:8885)

Edit the `itk_dev_user_management.notify_user_on_create` configuration setting
(in
[`config/packages/itk_dev_user_management.yaml`](config/packages/itk_dev_user_management.yaml))
to automatically notify users on creation.
