# Tutorial-Things-do-not-do-in-Python
Tutorial made from 'Coisas que NÃO devemos fazer ao programar em PYTHON - Codeshow #006' (CodeShow) by Marcus Mariano

---

## Introduction

Tutorial for things that you can not do in Python.

**Coisas que NÃO devemos fazer ao programar em PYTHON - Codeshow #006: [here.](https://www.youtube.com/watch?v=p4jWEC7vuKI&t=52s)**

---

## 01 - Follow style guides.

### Error - Wrong way.

```sh

def SumTwoNumbers(intA,intB):
  return intA+intB

results=dict(
  result10    =   SumTwoNumbers(5,5),
  result256   =   SumTwoNumbers(250,6)
)

```

### Ok - Right way.

- Use pep8

```sh

def sum_two_numbers(a, b):
    "Return sum of a and b."
    return a + b

results = {
    "result10": sum_two_numbers(5, 5),
    "result256": sum_two_numbers(250, 6)
}

```

### Better way

- Use pep8 and Type Annotations.

```sh

def sum_two_numbers(a: int, b: int) -> int:
    "Return sum of a and b."
    return a + b

results: Mapping[str, int] = {
    "result10": sum_two_numbers(5, 5),
    "result256": sum_two_numbers(250, 6)
}

```

---

## 02 - Let files open.

### Error - Wrong way.

```sh

data = open("~/Documents/data.csv")
do_somethings(data)

```

### Ok - Right way.

- Explicitly close the files.

```sh

data = open("~/Documents/data.csv")
do_somethings(data)
data.close()

```

### Better way

- Or preferably use Context Managers.

```sh

with open("~/Documents/data.csv") as data:
    do_somethings(data)

```

---

## 03 - Functions with multiple return types.

### Error - Wrong way.

```sh

def get_user(username):
    users = User(username=username).select()
    if users:
        return users.first()  # return type(User)
    # return  None - type(NoneType)

```

### Ok - Right way.

- Have only one return type. (use Exceptions)

```sh

def get_user(username):
    users = User(username=username).select()
    if not users:
        raise UserNotFoundError(f"{username} does not exist.")
    return users.first()

```

### Better way

- Have only one return type. Use Exceptions and Type Annotations.

```sh

def get_user(username: str) -> User:
    """
    Query user by its username.
    :param username: A str holding username
    :returns: User instance
    :raises: UserNotFoundError
    """
    users = User(username=username).select()
    if not users:
        raise UserNotFoundError(f"{username} does not exist.")
    return users.first()

```

---

## 04 - Use LBYL.

### Error - Wrong way.

```sh

# LBYL
# look Before You Leap
#pt-BR: Verifique antes de agir.

if Path(file_path).exists():  # Race Condition
    with open(file_path) as f:
        f.do_somethings()  # FileNotFoundError
else:
    print("The file do not exist!")

```

### Ok - Right way.

- Always prefer to use the EAFP.

```sh

# EAFP
# Easy Ask Forgiveness than Permission.
#pt-BR: É melhor pedir perdão do que permissão.

try:
    with open(file_path) as f:
        f.do_somethings()  
except FileNotFoundError:
    print("The file do not exist!")

```

---

## 05 - Mute all Exceptions.

### Error - Wrong way.

```sh

try:    
    do_somethings()  
except:
    pass  # Not pass all Exceptions.

```

### Ok - Right way.

```sh

try:    
    do_somethings()  
except CustomError:
    pass

```

### Better way

- Or preferably use a Context Manager!

```sh

with contextlib.suppress(CustomError):
    do_somethings()

```

---

## License

Code and documentation are available according to the GNU GENERAL PUBLIC LICENSE Version 3 (see [LICENSE](https://www.gnu.org/licenses/gpl.html)).
