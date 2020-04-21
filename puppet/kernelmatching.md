
case $::operatingsystem {

  'windows': {
  }

  'ubuntu': {
  }

  'Darwin': {
  }

  default: {
    notify { 'sorry, $::operatingsystem is not supported for ' }
  }

}
