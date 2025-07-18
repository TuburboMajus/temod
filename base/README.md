Here is the `README.md` for your Python package:

---

# Temod: Typed Entity Modeling for Object Definitions

Temod is a lightweight Python framework designed to facilitate the definition, validation, and manipulation of data entities using typed attributes, constraints, and graph-based object structures. It is ideal for developers building applications that require strict data modeling, data integrity enforcement, and customizable object graphs—especially for ORM-like systems, API modeling, or rule-based engines.

---

## 📦 Features

* ✅ Strongly typed attributes (`String`, `Email`, `Date`, `UUID`, `Enum`, etc.)
* 🔐 Built-in attribute-level validation with rich error classes
* 🔁 Automatic casting and formatting (e.g., string to date/time)
* 📎 Attribute constraints & logic composition (AND, OR, NOT, etc.)
* 📊 Join and Cluster objects for structured multi-entity modeling
* 🔄 Snapshot support for change tracking

---

## 🛠 Installation

```bash
pip install temod
```

---

## 🧱 Core Concepts

### 1. Attribute System

Each `Attribute` holds metadata (e.g., nullability, length, default value), enforces type and format, and supports force-casting and validation.

#### Basic Usage

```python
from temod.base.attribute import StringAttribute, EmailAttribute, DateAttribute

name = StringAttribute("name", non_empty=True, max_length=50)
email = EmailAttribute("email")
birthdate = DateAttribute("birthdate")
```

#### Validation

```python
name.set_value("Alice")     # ✅ Valid
email.set_value("invalid")  # ❌ Raises WrongEmailFormat
```

#### Custom Validators

```python
custom = StringAttribute("code", min_length=4, max_length=8, force_lower_case=True)
```

### 2. Attribute Types

* `StringAttribute`, `EmailAttribute`, `PhoneNumberAttribute`, `UUID4Attribute`
* `IntegerAttribute`, `RealAttribute`, `BooleanAttribute`
* `DateAttribute`, `TimeAttribute`, `DateTimeAttribute`
* `BytesAttribute`, `BCryptedAttribute` (e.g., for password hashing)
* `EnumAttribute`, `RangeAttribute`

---

### 3. Conditions

Logical constructs to express filter conditions or constraints:

```python
from temod.base.condition import And, Or, Equals, Superior

cond = And(
    Equals("user.name", "Alice"),
    Superior("user.age", 18)
)
```

You can nest conditions using `Not`, `And`, `Or`, and filtering types like `StartsWith`, `Contains`, etc.

---

### 4. Constraints

Declare logical or relational constraints between attributes:

```python
from temod.base.constraint import EqualityConstraint, RelaxedConstraint

constraint = EqualityConstraint(attr1, attr2)
relaxed = RelaxedConstraint(
    EqualityConstraint(attr1, attr2),
    EqualityConstraint(attr3, attr4)
)
```

You can also create bindings and complex nested constraints using `BindConstraint` and `NestedConstraint`.

---

### 5. Cluster System

Clusters wrap around graph structures and allow you to manage structured sets of related objects.

```python
from temod.base.cluster import Cluster

cluster = Cluster(graph=my_graph_structure)
print(cluster["user.name"])  # Access an attribute across the structure
```

You can convert clusters to plain Python dicts via `to_dict()`.

---

### 6. Joins

A `Join` merges multiple entities into a coherent unit, allowing unified access and nested referencing.

```python
from temod.base.join import Join

join = Join(UserEntity, OrderEntity)
print(join["user.name"])
join["order.total"] = 120.0
```

---

## 📤 Serialization

Entities and joins can be converted to dictionaries:

```python
data = join.to_dict()
```

---

## 🧪 Example: Defining a Secure User

```python
from temod.base.attribute import StringAttribute, EmailAttribute, BCryptedAttribute

class User:
    ATTRIBUTES = {
        "username": StringAttribute("username", non_empty=True),
        "email": EmailAttribute("email"),
        "password": BCryptedAttribute("password")
    }

user = User()
user.username.set_value("admin")
user.email.set_value("admin@example.com")
user.password.set_value("secret123")  # Automatically hashed
```

---

## ❗ Exception Handling

Temod uses fine-grained exception types for debugging and validation:

| Exception Class          | Trigger                          |
| ------------------------ | -------------------------------- |
| `NonNullableError`       | Null value on non-nullable field |
| `WrongEmailFormat`       | Invalid email                    |
| `OverMaxLengthError`     | String too long                  |
| `NumericBelowBoundError` | Integer/float below minimum      |
| `DateOverBoundError`     | Date out of allowed range        |
| `UnknownValueError`      | Enum or range value not allowed  |

Each exception carries the offending attribute and a code (e.g., `0x11`, `0x20`, etc.).

---

## 📚 Advanced Usage

### Dynamic Graph Expansion

```python
from temod.base.cluster import Cluster

structure = {
    "store_as": "user",
    "links": [
        {"store_as": "orders", "one_to_many": True, "condition": ...}
    ]
}
graph = Cluster.structure_to_graph(structure)
```

---

## 🔒 BCryptedAttribute

Hashes strings using bcrypt for secure password handling:

```python
password = BCryptedAttribute("password")
password.set_value("mypassword")  # stores bcrypt hash
assert password == "mypassword"   # ✅
```

---

## 📎 Design Philosophy

Temod is designed to be:

* **Modular**: Components are fully decoupled.
* **Declarative**: Define models as metadata-rich objects.
* **Strict**: Enforces data integrity upfront.
* **Extendable**: You can subclass and add custom logic easily.

---

## 🔮 Roadmap

* ✅ Attribute-level validators
* ✅ Entity serialization & diffing
* 🔜 Visual schema explorer

---

## 🧑‍💻 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

---

## 📜 License

MIT License © 2025

---
