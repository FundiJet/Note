# Golang 实现 bit array

> 学习 golang 过程中，简单实现 bit array

```go
package main

import (
	"bytes"
	"fmt"
)

// const target int = 32 << (^uint(0) >> 63) // 判断32/64位

// An IntSet is a set of small non-negative integers.
// Its zero value represents the empty set.
type IntSet struct {
	words []uint64 // uint
}

func main() {
	var x, y IntSet
	fmt.Printf("Begin x is %s\n", (&x).String())

	x.Add(1)
	x.Add(150)
	x.Add(200)
	x.Add(3452)
	x.Add(13452)

	// fmt.Println(x)          // 打印 IntSet
	fmt.Println(&x)            // 调用 *IntSet的String方法
	// fmt.Println(x.String()) // 调用 *IntSet的String方法

	y.Add(1)
	y.Add(1000)
	y.Add(200)
	// fmt.Println(y)
	fmt.Println(y.String())

	// z := x.DifferenceWith(&y)
	// fmt.Println(z)

	z := x.SymmetricDifference(&y)
	fmt.Println(z)

	for _, r := range x.Elems() {
		fmt.Println(r)
	}
}

func test1() {
	var x, y IntSet

	x.Add(1)
	x.Add(150)
	x.Add(13452)

	y.Add(100)
	y.Add(200)

	// x.UnionWith(&y)
	// fmt.Println(x.String())

	fmt.Println(x.Has(150), x.Has(123))

	fmt.Println(x.Len())

	x.Remove(13452)

	fmt.Println(x.String())

	// y.Clear()
	// fmt.Println(y)

	z := x.Copy()

	x.Add(23456)

	fmt.Println(x.String())
	fmt.Println(z.String())
	fmt.Printf("x address: %p\n", &x)
	fmt.Printf("z address: %p\n", &z)

	// x.AddAll(1561, 5648, 89746)
	// fmt.Println(x.String())

	r := x.IntersectWith(&y)
	fmt.Println(&r)
}

func (s *IntSet) String() string {
	var buf bytes.Buffer
	buf.WriteByte('{')

	for i, word := range s.words {
		if word == 0 {
			continue
		}

		for j := 0; j < 64; j++ {
			if word&(1<<uint(j)) != 0 {
				if buf.Len() > len("{") {
					buf.WriteByte(' ')
				}
				fmt.Fprintf(&buf, "%d", 64*i+j)
			}
		}
	}

	buf.WriteByte('}')
	return buf.String()
}

/*
 因为每一个字都有64个二进制位，所以为了定位x的bit位，我们用了x/64的商作为字的下标，并且用x%64得到的值作为这个字内的bit的所在位置。
*/
func (s *IntSet) Add(x int) {
	word, bit := x/64, uint(x%64)
	for word >= len(s.words) {
		s.words = append(s.words, 0)
	}
	s.words[word] |= 1 << bit
}

func (s *IntSet) Has(x int) bool {
	word, bit := x/64, uint(x%64)
	return word < len(s.words) && s.words[word]&(1<<bit) != 0
}

// 交集
func (s *IntSet) UnionWith(t *IntSet) {
	for i, tword := range t.words {
		if i < len(s.words) {
			s.words[i] |= tword
		} else {
			s.words = append(s.words, tword)
		}
	}
}

// 并集
func (s *IntSet) IntersectWith(x *IntSet) IntSet {
	var temp, other *IntSet
	if len(s.words) < len(x.words) {
		temp = s
		other = x
	} else {
		temp = x
		other = s
	}

	var result IntSet
	for i, word := range temp.words {
		if word == 0 {
			continue
		}

		if other.words[i]&word != 0 {
			for len(result.words) <= i {
				result.words = append(result.words, 0)
			}
			result.words[i] = word
		}
	}

	return result
}

// 差集：元素出现在 receiver 中，未出现在 x 中
func (s *IntSet) DifferenceWith(x *IntSet) *IntSet {
	result := &IntSet{}
	sLen, xLen := len(s.words), len(x.words)
	minLen := sLen
	if minLen > xLen {
		minLen = xLen
	}

	for i := 0; i < minLen; i++ {
		num := s.words[i] & x.words[i]
		result.words = append(result.words, s.words[i]^num)
	}

	for minLen < sLen {
		result.words = append(result.words, s.words[minLen])
		minLen++
	}
	return result
}

// 并差集：元素出现在A但没有出现在B，或者出现在B没有出现在A
func (s *IntSet) SymmetricDifference(x *IntSet) *IntSet {
	result := &IntSet{}
	sLen, xLen := len(s.words), len(x.words)
	minLen := sLen
	if minLen > xLen {
		minLen = xLen
	}
	for i := 0; i < minLen; i++ {
		result.words = append(result.words, s.words[i]^x.words[i])
	}

	for minLen < sLen {
		result.words = append(result.words, s.words[minLen])
		minLen++
	}
	return result
}

func (s *IntSet) Len() int {
	sum := 0
	for i, word := range s.words {
		if s.words[i] != 0 {
			for j := 0; j < 64; j++ {
				if word&(1<<uint(j)) != 0 {
					sum++
				}
			}
		}
	}
	return sum
}

func (s *IntSet) Elems() []interface{} {
	elems := []interface{}{}
	index := 0
	for i, word := range s.words {
		if s.words[i] != 0 {
			for j := 0; j < 64; j++ {
				if word&(1<<uint(j)) != 0 {
					elems = append(elems, 64*i+j)
					index++
				}
			}
		}
	}
	return elems
}

func (s *IntSet) Remove(x int) {
	if s.Has(x) {
		word, bit := x/64, uint(x%64)
		s.words[word] &^= uint64(1 << bit)
	}
}

func (s *IntSet) Clear() {
	*s = IntSet{}
}

func (s *IntSet) Copy() *IntSet {
	copy := IntSet{}
	copy.words = append(copy.words, s.words...)
	return &copy
}

func (s *IntSet) AddAll(x ...int) {
	for _, n := range x {
		s.Add(n)
	}
}

```

