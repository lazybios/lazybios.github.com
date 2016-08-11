---
layout: post
title: "RSpec::Core Cheat Sheet | Ruby"
date: "2016-08-11"
---

### The Basics
```ruby
RSpec.describe 'an array of animals' do
  let(:animal_array) { [:cat, :dog, :mule] }

  it 'has three animals' do
    expect(animal_array.size).to eq(3)
  end

  context 'mutation' do
    after { expect(animal_array.size).not_to eq(3) }

    it 'can have animals added' do
      animal_array << :cow
      expect(animal_array).to eq([:cat, :dog, :mule, :cow])
    end

    it 'can have animals removed' do
      animal_array.pop
      expect(animal_array).to eq([:cat, :dog])
    end
  end
end
```

### Let
每次执行测试用例的时候都会被重新创建
```ruby
RSpec.describe 'Uses of `let`' do
  let(:random_number) { rand }
  let(:lazy_creation_time) { Time.now }
  let!(:eager_creation_time) { Time.now }

  it 'memoizes values' do
    first = random_number
    second = random_number
    expect(first).to be(second)
  end

  it 'creates the value lazily' do
    start_of_test = Time.now
    expect(lazy_creation_time).to be > start_of_test
  end

  it 'creates the value eagerly using `let!`' do
    start_of_test = Time.now
    expect(eager_creation_time).to be < start_of_test
  end
end
```

### Subject
```ruby
RSpec.describe Array do
  it 'provides methods based on the `RSpec.describe` argument' do
    # described_class = Array
    expect(described_class).to be(Array)
    # subject = described_class.new
    expect(subject).to eq(Array.new)
    # is_expected = expect(subject)
    is_expected.to eq(Array.new)
  end

  context 'explicitly defined subject' do
    # subject can be manually defined
    subject { [1,2,3] }
    it 'is not empty' do
      is_expected.not_to be_empty
    end
  end

  context 'can be named' do
    # you can provide a name, just like `let`
    subject(:bananas) { [4,5,6] }
    it 'can be called by name' do
     expect(bananas.first).to eq(4)
    end
  end
end
```

### Hooks
```ruby
RSpec.describe 'Hooks' do
  order = []

  before(:all) { order << 'before(:all)' }
  before       { order << 'before' }
  after        { order << 'after' }
  after(:all)  { order << 'after(:all)'; puts order }

  around do |test|
    order << 'around, pre'
    test.call
    order << 'around, post'
  end

  it 'runs first test' do
    order << 'first test'
  end

  it 'runs second test' do
    order << 'second test'
  end
end
```
执行结果
```ruby
Execution order for the tests above:

before(:all)
	around, pre
		before
		first test
		after
	around, post
	around, pre
		before
		second test
		after
	around, post
after(:all)
```

### Skipping
```ruby
RSpec.describe 'Ways to skip tests' do
  it 'is skipped because it has no body'

  skip 'uses `skip` instead of `it`' do
  end

  xit 'uses `xit` instead of `it`' do
  end

  it 'has `skip` in the body' do
    skip
  end

  xcontext 'uses `xcontext` to skip a group of tests' do
    it 'wont run' do; end
    it 'wont run either' do; end
  end
end
```

### Pending
```ruby
RSpec.describe 'Ways to mark failing tests as "pending"' do
  pending 'has a failing expectation' do
    expect(1).to eq(2)
  end

  it 'has `pending` in the body' do
    pending('reason goes here')
    expect(1).to eq(2)
  end

  pending 'tells you if a pending test has been fixed' do
    # Pending tests are supposed to fail. This test passes,
    # so RSpec will give an error saying that this pending
    # test has been fixed.
    expect(2).to eq(2)
  end
end
```

### Alternate Syntax
`describe`与`it`的别名方法，只是为了让DSL看起来更符合语境，读起来易懂一些。
+ Groups: `context, example_group, describe`
+ Examples: `it, example, specify`

```ruby
RSpec.describe 'Alternate syntax' do
  example_group 'alernative to "context"' do
    example 'alternative to "it"' do
      expect(2).to eq(2)
    end
  end

  describe 'alternative to "context"' do
    specify 'alternative to "it"' do
      expect(2).to eq(2)
    end
  end
end
```

### Defining Methods
在测试用例中自定义辅助方法
```ruby
RSpec.describe 'Defining methods' do
  def my_helper_method(name)
    "Hello #{name}, you just got helped!"
  end

  it 'uses my_helper_method' do
    message = my_helper_method('Susan')
    expect(message).to eq('Hello Susan, you just got helped!')
  end

  context 'within a context group' do
    it 'can still use my_helper_method' do
      message = my_helper_method('Tom')
      expect(message).to eq('Hello Tom, you just got helped!')
    end
  end
end
```

### Shared Examples (Shared Tests)
通过include的方式重用部分测试用例
```ruby
RSpec.shared_examples 'acts like non-nil array' do
  it 'has a size' do
    expect(subject.size).to be > 0
  end

  it 'has has non-nil values for each index' do
    subject.size.times do |index|
      expect(subject[index]).not_to be_nil
    end
  end
end

RSpec.describe 'A real array' do
  include_examples 'acts like non-nil array' do
    subject { ['zero', 'one', 'two'] }
  end
end

RSpec.describe 'A hash with integer keys' do
  include_examples 'acts like non-nil array' do
    subject { Hash[0 => 'zero', 1 => 'one', 2 => 'two'] }
  end
end
```

### Shared Context
通过include的方式重用执行环境的上下文(Hooks, Methods以及Lets)

```ruby
RSpec.shared_context 'test timing' do
  around do |test|
    start_time = Time.now
    test.call
    end_time = Time.now
    puts "Test ran in #{end_time - start_time} seconds"
  end
end

RSpec.describe 'big array' do
  include_context 'test timing'

  it 'has lots of elements' do
    big_array = (1..1_000_000).to_a
    expect(big_array.size).to eq(1_000_000)
  end
end
```

-完-

### 参考引用
+ [http://t.cn/RtO8ULM]()
