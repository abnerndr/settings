### 1.0 install

```bash
yarn add sqids
```

### 1.1 implement

```typescript
import { Sqids } from 'sqids'

const sqids = new Sqids()
```

### 1.2 encripty

```typescript
import { Entity, PrimaryColumn, Column } from 'typeorm'
import { Sqids } from 'sqids'

@Entity('accounts')
class Account {
  @PrimaryColumn()
  bid: string

  public get sqid() : string {
    const sqids = new Sqids({
    minLength: 10,
    })
    const id = sqids.encode(this.bid)
    return id 
  }
}
```

### 1.2 decripty

```typescript
const sqid = '86Rf07xd4z'
const bid = sqids.decode(sqid)
return bid // 1
```